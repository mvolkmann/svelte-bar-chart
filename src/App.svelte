<script>
  import {tweened} from 'svelte/motion';
  import BarChart from './BarChart.svelte';

  // Full size of the svg element.
  const height = 300;
  const width = 400;

  const labels = [
    'apple',
    'banana',
    'cherry',
    'date',
    'grape',
    'mango',
    'peach',
    'raspberry',
    'strawberry',
    'tangerine',
    'watermelon'
  ];

  const data = labels.map((label, index) => ({
    colorIndex: index,
    height: tweened(0, {duration: 500}),
    label
  }));

  // This returns a random integer from 1 to max inclusive.
  const random = max => Math.floor(Math.random() * max + 1);

  // This returns an array of objects taken from allData.
  // A "value" property with a random value from 1 to 10
  // is added to each object.
  function setRandomData() {
    const count = random(labels.length);
    const shuffledLabels = labels.sort(() => 0.5 - Math.random());
    const labelSet = new Set(shuffledLabels.slice(0, count));

    for (const item of data) {
      item.value = labelSet.has(item.label) ? random(10) : 0;
    }

    data = data; // trigger reactivity
  }

  setRandomData();
</script>

<BarChart {data} {height} {width} />
<div>
  <button on:click={setRandomData}>Update</button>
</div>

<style>
  :global(body) {
    background-color: linen;
  }
</style>
