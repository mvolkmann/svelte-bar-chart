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
  const SPACE = 10; // between bars

  const usableHeight = height - TOP_PADDING - BOTTOM_PADDING;
  const usableWidth = width - LEFT_PADDING - RIGHT_PADDING;

  let maxValue = 0;

  // We only want to draw a bar for items whose value is not zero.
  $: dataStaying = data.filter(d => d.value);

  $: barWidth = Math.round(usableWidth / dataStaying.length);

  $: maxValue = dataStaying.reduce((acc, d) => Math.max(acc, d.value), 0);

  // Change the value of the height tweened store for each item.
  $: for (const item of data) {
    item.height.set((item.value / maxValue) * usableHeight);
  }
</script>

<svg {height} {width}>
  {#each dataStaying as obj, index (obj.label)}
    <Bar
      bottomY={TOP_PADDING + usableHeight}
      data={obj}
      padding={SPACE / 2}
      width={barWidth - SPACE}
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
