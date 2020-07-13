<script>
  import {flip} from 'svelte/animate';
  import XAxis from './XAxis.svelte';
  import YAxis from './YAxis.svelte';

  export let data;
  export let height;
  export let width;

  const BOTTOM_PADDING = 60;
  const LEFT_PADDING = 30;
  const RIGHT_PADDING = 10;
  const TOP_PADDING = 10;

  const usableHeight = height - TOP_PADDING - BOTTOM_PADDING;
  const usableWidth = width - LEFT_PADDING - RIGHT_PADDING;

  // These colors are taken from d3.schemePaired.
  const colors = [
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

  const space = 10; // between bars

  $: barWidth = Math.round(usableWidth / data.length);
  $: maxValue = data.reduce((acc, obj) => Math.max(acc, obj.value), 0);

  const getHeight = data =>
    Math.round((data.value / maxValue) * (usableHeight - 1));

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

<svg {height} {width}>
  {#each data as obj, index (obj.label)}
    <g
      animate:flip
      transform={`translate(${LEFT_PADDING + barWidth * index}, 0)`}>
      <rect
        height={getHeight(obj)}
        width={barWidth - space}
        x={space / 2}
        y={TOP_PADDING + usableHeight - getHeight(obj)}
        style={`fill: ${colors[index]}`} />
      <text
        x={space / 2 + (barWidth - space) / 2}
        y={TOP_PADDING + usableHeight - getHeight(obj) + 20}
        style={`fill: ${getTextColor(colors[index])}`}>
        {obj.value}
      </text>
    </g>
  {/each}

  <YAxis
    height={usableHeight}
    {maxValue}
    width={LEFT_PADDING}
    x={0}
    y={TOP_PADDING} />
  <XAxis
    {data}
    width={usableWidth}
    x={LEFT_PADDING}
    y={TOP_PADDING + usableHeight} />
</svg>

<style>
  svg {
    background-color: white;
  }

  rect {
    stroke: black;
    stroke-width: 1;
  }

  text {
    text-anchor: middle;
  }
</style>
