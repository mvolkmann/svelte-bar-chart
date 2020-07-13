<script>
  export let height;
  export let width;
  export let maxValue;
  export let x;
  export let y;

  const spaceBeforeTick = 3;
  const tickWidth = 5;

  // Create an array with values from zero to max
  // that will be used as the tick values on the y axis.
  $: tickValues = Array.from(Array(maxValue + 1).keys());

  $: delta = height / maxValue;

  const getY = value => (maxValue - value) * delta;
</script>

<g transform={`translate(${x}, ${y})`}>
  <line x1={width} y1={height} x2={width} y2="0" stroke="black" />
  {#each tickValues as value}
    <line
      x1={width - tickWidth}
      y1={getY(value)}
      x2={width}
      y2={getY(value)}
      stroke="black" />
    <text x={width - tickWidth - spaceBeforeTick} y={getY(value)}>{value}</text>
  {/each}
</g>

<style>
  text {
    alignment-baseline: central;
    font-size: 0.7rem;
    text-anchor: end;
  }
</style>
