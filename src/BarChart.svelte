<script>
  import Bar from './Bar.svelte';
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

  let maxValue = 0;

  $: dataSubset = data.filter(d => d.value);
  $: barWidth = Math.round(usableWidth / dataSubset.length);
  $: maxValue = dataSubset.reduce((acc, d) => Math.max(acc, d.value), 0);

  $: for (const item of data) {
    item.height.set((item.value / maxValue) * usableHeight);
  }

  function getHeight(obj) {
    const height = maxValue
      ? Math.round((obj.value / maxValue) * (usableHeight - 1))
      : 0;
    obj.height.set(height);
    return obj.height; // a store
  }

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
  {#each dataSubset as obj, index (obj.label)}
    <Bar
      color={colors[obj.colorIndex]}
      heightStore={obj.height}
      textColor={getTextColor(colors[obj.colorIndex])}
      {usableHeight}
      width={barWidth - space}
      value={obj.value}
      x={LEFT_PADDING + barWidth * index} />
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
</style>
