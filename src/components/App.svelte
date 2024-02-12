<script>
import * as d3 from 'd3';
import { onMount } from 'svelte';

let data = [];

onMount(async () => {
    const res = await fetch(
      'https://raw.githubusercontent.com/mf02511/Median-Earnings-by-College-Major/main/src/components/major_earnings.csv',
    );
    const csv = await res.text();
    await d3.csvParse(csv, (d, i, columns) => {
        let row = {}
        for (let j = 0; j < 27; j++) {
            row[columns[j]] = d[columns[j]]
        }
        data.push(row)
    });
    data=data;
});

$: console.log(data);


</script>

<main>
    <h1>Median Earnings by College Majors</h1>
    <p></p>
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
