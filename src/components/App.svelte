<script>
import * as d3 from 'd3';
import { onMount } from 'svelte';

let data = [];

onMount(async () => {
    const res = await fetch('gender.csv');
    const csv = await res.text();
    data = d3.csvParse(csv, d3.autoType);
});
$: console.log(data)
</script>


<main>
    <h1>Median Earnings by College Majors</h1>
</main>


<style>
  @import url('https://fonts.googleapis.com/css2?family=Assistant:wght@200;400;600;700;800&display=swap');
    main {
        text-align: center;
        font-family: 'Assistant', sans-serif;
    }

    h1 {
        font-size: 72px;
        font-weight: 200;
    }
</style>
