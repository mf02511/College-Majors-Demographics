<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import GenderDist from './GenderDist.svelte';

    let genderData = [];

    onMount(async () => {
        const res = await fetch('gender.csv');
        const csv = await res.text();
        genderData = d3.csvParse(csv, d3.autoType);
    });
</script>


<main>
    <h1>Median Earnings by College Majors</h1>
    <GenderDist {genderData} />
</main>


<style>
    @import url('https://fonts.googleapis.com/css2?family=Assistant:wght@200;400;600;700;800&display=swap');

    *,
    *::before,
    *::after {
        margin: 0;
        padding: 0;
    }
    main {
        text-align: center;
        font-family: 'Assistant', sans-serif;
        margin: 0;
        padding: 0;
    }

    h1 {
        font-size: 30px;
        font-weight: 800;
    }

</style>
