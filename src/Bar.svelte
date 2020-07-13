<script>
  // We cannot do all of this in the BarChart component
  // because we want to auto-subscribe to the height stores and
  // that can only be done when the store is in a top-level variable,
  // not when it is a property of an object.
  export let color;
  export let heightStore;
  export let usableHeight;
  export let value;
  export let width;
  export let x;

  const SPACE = 10; // between bars
  const TOP_PADDING = 10;

  // This returns a text color to use on a given background color.
  function getTextColor(bgColor) {
    // Convert the hex background color to its decimal components.
    const red = parseInt(bgColor.substring(1, 3), 16);
    const green = parseInt(bgColor.substring(3, 5), 16);
    const blue = parseInt(bgColor.substring(5, 7), 16);

    // Compute the "relative luminance".
    const luminance = (0.2126 * red + 0.7152 * green + 0.0722 * blue) / 255;

    // Use dark text on light backgrounds and vice versa.
    return luminance > 0.5 ? 'black' : 'white';
  }
</script>

<g transform={`translate(${x}, 0)`}>
  <rect
    height={$heightStore}
    {width}
    x={SPACE / 2}
    y={TOP_PADDING + usableHeight - $heightStore}
    style={`fill: ${color}`} />
  {#if value}
    <text
      x={SPACE / 2 + width / 2}
      y={TOP_PADDING + usableHeight - $heightStore + 20}
      style={`fill: ${getTextColor(color)}`}>
      {value}
    </text>
  {/if}
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
