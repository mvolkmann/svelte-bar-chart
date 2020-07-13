<script>
  export let color;
  export let heightStore;
  export let textColor;
  export let usableHeight;
  export let value;
  export let width;
  export let x;

  const SPACE = 10; // between bars
  const TOP_PADDING = 10;
</script>

<g transform={`translate(${x}, 0)`}>
  <rect
    height={$heightStore}
    {width}
    x={SPACE / 2}
    y={TOP_PADDING + usableHeight - $heightStore}
    style={`fill: ${color}`} />
  <text
    x={SPACE / 2 + width / 2}
    y={TOP_PADDING + usableHeight - $heightStore + 20}
    style={`fill: ${textColor}`}>
    {value}
  </text>
</g>

<style>
  rect {
    stroke: black;
    stroke-width: 1;
  }

  text {
    text-anchor: middle;
  }
</style>
