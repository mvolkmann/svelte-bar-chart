<script>
  export let data;
  export let width;
  export let x;
  export let y;

  const spaceAfterTick = 3;
  const tickHeight = 5;

  $: dataSubset = data.filter(d => d.value);
  $: halfBarWidth = width / dataSubset.length / 2;
  $: delta = width / dataSubset.length;

  $: getTransform = index =>
    `translate(5 20) ` +
    `rotate(-45 ${getX(index)} ${tickHeight + spaceAfterTick})`;

  $: getX = index => index * delta + halfBarWidth;
</script>

<g transform={`translate(${x}, ${y})`}>
  <line x1="0" y1="0" x2={width} y2="0" stroke="black" />
  {#each dataSubset as obj, index}
    <line
      x1={getX(index)}
      y1="0"
      x2={getX(index)}
      y2={tickHeight}
      stroke="black" />
    <text
      x={getX(index)}
      y={tickHeight + spaceAfterTick}
      transform={getTransform(index)}>
      {obj.label}
    </text>
  {/each}
</g>

<style>
  text {
    font-size: 0.7rem;
    text-anchor: middle;
  }
</style>
