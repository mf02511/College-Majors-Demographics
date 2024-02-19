<script>
	import * as d3 from 'd3';

	export let genderData;

	const width = 1400;
	const height = 1200;
	const marginTop = 40;
	const marginRight = 40;
	const marginBottom = 100;
	const marginLeft = 140;

	let gx;
	let gy;

	let hovered = -1;
	let recorded_mouse_position = {
		x: 0, y: 0
	};


	const subgroups = ['Male', 'Female'];

	$: y = d3
		.scaleBand()
		.range([marginTop, height - marginBottom])
		.domain(genderData.map(function(d) {return d.Degree_Abv;}))
		.padding(1);
	$: x = d3
		.scaleLinear()
		.range([0, width - marginRight - marginLeft])
		.domain([0, 3839000]);
	$: d3.select(gy).call(d3.axisLeft(y));
	$: d3.select(gx).call(d3.axisBottom(x));
	$: stackedData = d3.stack()
		.keys(subgroups)
		(genderData);
	$: color = d3.scaleOrdinal()
		.domain(subgroups)
		.range(['#7685c0', '#da7454'])

</script>

<div class="gender-dist-plot">
	<svg
		{width}
		{height}
		viewBox="0 0 {width} {height}"
		style="max-width: 100%; height: auto;"
	>
		<g
			bind:this={gy} 
			transform="translate({marginLeft}, 0)"
			class='y-axis'
		>
			<text
				x=-{marginLeft - 40}
				y={marginTop - 10}
				dy="0.32em"
				fill="#000"
				font-weight=700
				font-size=25px
				text-anchor="start"
			>
				Major
			</text>
		</g>
		<g 
			bind:this={gx} 
			transform="translate({marginLeft}, {height - marginBottom})"
			class='x-axis'
		>
			<text
				x={(width / 2) - marginLeft}
				y={marginBottom/2 + 20}
				fill="#000"
				font-weight=700
				font-size=25px
				text-anchor="start"
			>
				Number of People
			</text>
		</g>
		<g 
		class='bar'
		>
			{#each stackedData as genders}
				{#each genders as d, i}
					<rect
						x={x(d[0]) + marginLeft}
						y={y(d.data.Degree_Abv) - 15}
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

		<g class='legend-box'>
			<rect
				x=1170
				y={marginTop - 20}
				width=160
				height=90
				fill={'#e7e6eb'}
				rx=4px
				ry=4px
			/>
		</g>

		<g class='legend-dots'>
			{#each subgroups as key, i}
				<rect
					x=1180
					y={marginTop + (i * 30)}
					width=20
					height=20
					fill={color(key)}
				/>
			{/each}
		</g>

		<g class='legend-labels'>
			{#each subgroups as key, i}
				<text
					x={1210}
					y={marginTop + (i * 30) + 15}
					width=20
					height=20
					font-weight=600
					font-size=20px
				>
					{key}
				</text>
			{/each}
		</g>

	</svg>

	<div
		class={hovered === -1 ? "tooltip-hidden": "tooltip-visible"}
		style="left: {recorded_mouse_position.x + 20}px; top: {recorded_mouse_position.y + 20}px"	
	>
		{#if hovered !== -1}
			<u><b>{genderData[hovered].Degree}</b></u>
			<br>
			males:  {genderData[hovered].Male} ({(genderData[hovered].Male * 100.0 / 
			(genderData[hovered].Male + genderData[hovered].Female)).toFixed(1)}%)
			<br>
			females:  {genderData[hovered].Female} ({(genderData[hovered].Female * 100.0 / 
			(genderData[hovered].Male + genderData[hovered].Female)).toFixed(1)}%)
			<br>
			________________________
			total: {genderData[hovered].Male + genderData[hovered].Female}
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
		width: 220px;
		color: black;
		position: absolute;
		text-align: left;
		padding: 10px;
	}

	.y-axis {
		font: 16px sans-serif;
		font-family: "Assistant", sans-serif;
		font-weight: 600;
	}

	.x-axis {
		font: 14px sans-serif;
		font-family: "Assistant", sans-serif;
		font-weight: 600;
	}
</style>
