<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import GenderDist from './GenderDist.svelte';
    import RaceDist from './RaceDist.svelte';

    let genderData = [];

    onMount(async () => {
        const resGender = await fetch('gender.csv');
        const csvGender = await resGender.text();
        genderData = d3.csvParse(csvGender, d3.autoType);
    });

    let raceData = [];

    onMount(async () => {
        const resRace = await fetch('race.csv');
        const csvRace = await resRace.text();
        raceData = d3.csvParse(csvRace, d3.autoType);
    });
</script>


<main>
    <h1>Median Earnings by College Majors</h1>
    <RaceDist {raceData} />
</main>


<style>
    @import url('https://fonts.googleapis.com/css2?family=Assistant:wght@200;400;600;700;800&display=swap');
    *,
    *::before,
    *::after {
        margin: 0;
        padding: 0;
    }
    :global(body){
        margin: 0;

    }
    main {
        text-align: center;
        font-family: 'Assistant', sans-serif;
        margin: 0;
        padding: 0;
    }

    h1 {
        font-size: 50px;
        font-weight: 800;
    }
    GenderDist {
        margin: 0;
        padding: 0;
        border: 0;
    }

</style>
