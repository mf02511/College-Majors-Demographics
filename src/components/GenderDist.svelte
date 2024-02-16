<script>
	import * as d3 from 'd3';

	export let genderData;

	const width = 1300;
	const height = 1400;
	const marginTop = 20;
	const marginRight = 40;
	const marginBottom = 30;
	const marginLeft = 90;

	let gx;
	let gy;

	let hovered = -1;
	let recorded_mouse_position = {
		x: 0, y: 0
	};


	const subgroups = ['Male', 'Female'];

	$: y = d3
		.scaleBand()
		.range([0, height - marginBottom])
		.domain(genderData.map(function(d) {return d.Degree;}))
		.padding(1);
	$: x = d3
		.scaleLinear()
		.range([0, width - marginRight])
		.domain([0, 3839000]);
	$: d3.select(gy).call(d3.axisLeft(y));
	$: d3.select(gx).call(d3.axisBottom(x));
	$: stackedData = d3.stack()
		.keys(subgroups)
		(genderData);
	$: color = d3.scaleOrdinal()
		.domain(subgroups)
		.range(['#7faaf0', '#f0a1c7'])

</script>

<div class="gender-dist-plot">
	<svg
		{width}
		{height}
		viewBox="0 0 {width} {height}"
		style="max-width: 100%; height: auto;"
	>
		<g bind:this={gy} transform="translate({marginLeft}, 0)"/>
		<g bind:this={gx} transform="translate({marginLeft}, {height - marginBottom})"/>
		<g 
		class='bar'
		>
			{#each stackedData as genders}
				{#each genders as d, i}
					<rect
						x={x(d[0]) + marginLeft}
						y={y(d.data.Degree) - 15}
						width={x(d[1] - d[0])}
						height={30}
						fill={color(genders.key)}
						opacity={i === hovered ? 0.5: 1}
						on:mouseover={(event) => {
							hovered = i;
							recorded_mouse_position = {
								x: event.pageX,
								y: event.pageY
							}
						}}
						on:mouseout={(event) => {hovered = -1;}}
					/>
				{/each}
			{/each}
		</g>

	</svg>

	<div
		class={hovered === -1 ? "tooltip-hidden": "tooltip-visible"}
		style="left: {recorded_mouse_position.x + 20}px; top: {recorded_mouse_position.y + 20}px"	
	>
		{#if hovered !== -1}
			<b>{genderData[hovered].Degree}</b>
			<br>
			number of males: {genderData[hovered].Male} ({(genderData[hovered].Male * 100.0 / 
			(genderData[hovered].Male + genderData[hovered].Female)).toFixed(1)}%)
			<br>
			number of females: {genderData[hovered].Female} ({(genderData[hovered].Female * 100.0 / 
			(genderData[hovered].Male + genderData[hovered].Female)).toFixed(1)}%)
		{/if}
	</div>
	
</div>

<style>
	.tooltip-hidden {
		visibility: hidden;
		font-family: "Assistant", sans-serif;
		width: 500px;
		position: absolute;
	}

	.tooltip-visible {
		font: 18px sans-serif;
		font-family: "Assistant", sans-serif;
		font-weight: 600;
		visibility: visible;
		background-color: #e7e6eb;
		border-radius: 5px;
		width: 300px;
		color: black;
		position: absolute;
		text-align: left;
		padding: 10px;
	}
</style>
