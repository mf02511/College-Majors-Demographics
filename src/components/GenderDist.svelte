<script>
    import * as d3 from 'd3';

    export let genderData;

    const width = 1300;
    const height = 1400;
    const marginTop = 20;
    const marginRight = 40;
    const marginBottom = 30;
    const marginLeft = 150;

    let gx;
    let gy;

    $: y = d3
        .scaleBand()
        .range([0, height - marginBottom])
        .domain(genderData.map(function(d) {return d.Degree;}))
        .padding(2);
    
    $: d3.select(gy).call(d3.axisLeft(y));
</script>

<div class = "gender-dist-plot">
    <svg 
        {width} 
        {height} 
        viewBox="0 0 {width} {height}"
        style = "max-width: 100%; height: auto;">

        <g bind:this={gy} transform="translate({marginLeft}, 0)"/>
    </svg>
</div>