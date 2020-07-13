<script>
  export let bottomY;
  export let data;
  export let padding;
  export let width;
  export let x;

  // These colors are taken from d3.schemePaired.
  const COLORS = [
    '#a6cee3',
    '#1f78b4',
    '#b2df8a',
    '#33a02c',
    '#fb9a99',
    '#e31a1c',
    '#fdbf6f',
    '#ff7f00',
    '#cab2d6',
    '#6a3d9a',
    '#ffff99',
    '#b15928'
  ];

  $: color = COLORS[data.colorIndex];
  $: heightStore = data.height;

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
    x={padding}
    y={bottomY - $heightStore}
    style={`fill: ${color}`} />
  {#if data.value}
    <text
      x={padding + width / 2}
      y={bottomY - $heightStore + 20}
      style={`fill: ${getTextColor(color)}`}>
      {data.value}
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
