<script>
  export let x;
  export let y;
  export let mx;
  export let my;
  export let sz;
  export let color;

  let max_eye_movement = 0.3 * sz;

  $: dx = mx !== null ? (mx - x) : 0
  $: dy = my !== null ? (my - y) : 0
  $: dl = Math.max(0.000001, Math.sqrt(dx*dx + dy*dy))
  $: displacement = Math.min(max_eye_movement, dl)
  $: rx = x + dx/dl * displacement
  $: ry = y + dy/dl * displacement
</script>

<g>
  <circle class="eye1" cx={x} cy={y} r={sz} />
  <circle class="eye2" cx={rx} cy={ry} r={sz * 0.5} style="fill: {color}"/>
  <circle class="eye3" cx={rx} cy={ry} r={sz * 0.2}/>
</g>

<style>
  .eye1 {
    fill: white;
    stroke: black;
    stroke-width: 3px;
  }
  .eye2 {
    stroke: black;
    stroke-width: 1px;
  }
  .eye3 {
    fill: black;
  }
</style>
