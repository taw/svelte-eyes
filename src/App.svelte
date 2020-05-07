<script>
	import Eye from "./Eye.svelte";

	function random_color() {
    let h = Math.random() * 360
    let s = Math.round(50 + Math.random() * 50)
    let l = Math.round(30 + Math.random() * 40)
    return `hsl(${h}, ${s}%, ${l}%)`
	}

  function eye_distance(eye1, eye2) {
    let dx = eye1.x - eye2.x;
    let dy = eye1.y - eye2.y;
		return Math.sqrt((dx * dx) + (dy * dy))
	}

	function can_place_eye(new_eye) {
    return eyes.every(eye =>
			eye_distance(eye, new_eye) >= eye.sz + new_eye.sz + 5
		)
	}

  function onmousemove(event) {
    let svg = document.getElementById("eyes")
    let rect = svg.getBoundingClientRect()
    mx = event.pageX - rect.x
    my = event.pageY - rect.y
	}

	let eyes = [];
	let wh = window.innerHeight;
	let ww = window.innerWidth;
	// init it to something random if mouse starts outside window
	let mx = Math.random() * ww;
	let my = Math.random() * wh;

	[...Array.from({ length: 1000 })].forEach(_ => {
		let sz = 20 + Math.random() * 60
		let x = sz + Math.random() * (ww - 2 * sz)
		let y = sz + Math.random() * (wh - 2 * sz)
		let color = random_color()
		let new_eye = {x, y, sz, color}
    if (can_place_eye(new_eye)) {
		  eyes.push(new_eye)
		}
	});
</script>

<svg id="eyes" on:mousemove={onmousemove}>
	{#each eyes as {x,y,sz,color}}
  	<Eye {x} {y} {mx} {my} {sz} {color}/>
	{/each}
</svg>

<style>
  svg {
    width: 100vw;
    height: 100vh;
    display: block;
    background-color: #aaa;
  }
</style>

<!--
tag App
  def mount
    schedule(raf: true)

@import 'normalize-scss';
@include normalize();
}-->
