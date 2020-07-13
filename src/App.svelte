<script>
  import BarChart from './BarChart.svelte';

  // Full size of the svg element.
  const height = 300;
  const width = 400;

  const allData = [
    {label: 'apple', colorIndex: 1},
    {label: 'banana', colorIndex: 2},
    {label: 'cherry', colorIndex: 3},
    {label: 'date', colorIndex: 4},
    {label: 'grape', colorIndex: 5},
    {label: 'mango', colorIndex: 6},
    {label: 'peach', colorIndex: 7},
    {label: 'raspberry', colorIndex: 8},
    {label: 'strawberry', colorIndex: 9},
    {label: 'tangerine', colorIndex: 10},
    {label: 'watermelon', colorIndex: 11}
  ];

  let data = [];

  // This returns a random integer from 1 to max inclusive.
  const random = max => Math.floor(Math.random() * max + 1);

  // This returns an array of objects taken from allData.
  // A "value" property with a random value from 1 to 10
  // is added to each object.
  function getRandomData() {
    const count = random(allData.length);
    const shuffled = allData.sort(() => 0.5 - Math.random());
    const data = shuffled.slice(0, count);
    data.sort((d1, d2) => d1.label.localeCompare(d2.label));
    for (const item of data) {
      item.value = random(10);
    }
    return data;
  }

  function updateData() {
    data = getRandomData();
  }

  updateData();
</script>

<BarChart {data} {height} {width} />
<div>
  <button on:click={updateData}>Update</button>
</div>

<style>
  :global(body) {
    background-color: linen;
  }
</style>
