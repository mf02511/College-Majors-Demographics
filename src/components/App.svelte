<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import GenderDist from './GenderDist.svelte';
    import RaceDist from './RaceDist.svelte';
    import AgeDist from './AgeDist.svelte';

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

    let ageData = [];

    onMount(async () => {
        const resAge = await fetch('age.csv');
        const csvAge = await resAge.text();
        ageData = d3.csvParse(csvAge, d3.autoType);
    });

    let display = 0;

</script>


<main>
    <h1>College Majors in the U.S. Workforce by Demographic (2022)</h1>
    <h2>data source: <a href="https://www.census.gov/data/tables/2022/demo/educational-attainment/acs-detailed-tables.html">2022 ACS Detailed Field of Degree and Median Annual Earnings</a><h2>
    <button 
        on:click={(event) => {display = 0;}} 
        class="button"
    >
         Gender 
    </button>
    <button 
        on:click={(event) => {display = 1;}} 
        class="button"
    >
         Race 
    </button>
    <button 
        on:click={(event) => {display = 2;}} 
        class="button"
    >
        Age
    </button>

    {#if display == 0}
        <GenderDist {genderData}/>
    {:else if display == 1}
        <RaceDist {raceData}/>
    {:else}
        <AgeDist {ageData}/>
    {/if}

</main>


<style>
    @import url('https://fonts.googleapis.com/css2?family=Assistant:wght@200;400;600;700;800&display=swap');
    *,
    *::before,
    *::after {
        margin: 0;
        padding: 0;
        border: 0;
    }

    :global(body){
        margin: 0;
        padding: 0;
        border: 0;
        width: 100%;
        height: 100%;
        overflow-x: hidden; 

    }
    main {
        text-align: center;
        font-family: 'Assistant', sans-serif;
        margin: 0;
        padding: 0;
        border: 0;
        width: 100%;
        height: 100%;
    }

    .button {
        font-size: 24px;
        font-family: 'Assistant', sans-serif;
        background-color: #fce3bd;
        border: 2px solid #ffb45e;
        padding: 3px 10px;
        margin: 4px 10px;
        border-radius: 4px;
    }
    .button:hover{
        font-weight: 600;
    }
    .button:focus{
        font-weight: 600;
        border: 2px solid #ffb45e;
        background-color: #ffb45e;
    }


    h1 {
        font-size: 35px;
        font-weight: 800;
        margin: 10px;
    }

    h2 {
        font-size: 18px;
        font-weight: 400;
        margin: 10px;
    }
</style>
