<script>
	import * as d3 from 'd3';

	export let ageData;

	const width = 1300;
	const height = 1200;
	const marginTop = 20;
	const marginRight = 40;
	const marginBottom = 100;
	const marginLeft = 110;

	let gx;
	let gy;

	let hovered = -1;
	let recorded_mouse_position = {
		x: 0, y: 0
	};


	const subgroups = ['25', '35', '45', '55'];
	const labels = ['25-34 years old', '35-44 years old', '45-54 years old', 
			'55-64 years old'
			]

	$: y = d3
		.scaleBand()
		.range([0, height - marginBottom])
		.domain(ageData.map(function(d) {return d.Degree_Abv;}))
		.padding(1);
	$: x = d3
		.scaleLinear()
		.range([0, width - marginRight])
		.domain([0, 3839000]);
	$: d3.select(gy).call(d3.axisLeft(y));
	$: d3.select(gx).call(d3.axisBottom(x));
	$: stackedData = d3.stack()
		.keys(subgroups)
		(ageData);
	$: color = d3.scaleOrdinal()
		.domain(subgroups)
		.range(['#95a0c9', '#7685c0', '#4d61b0', '#3349a3'])

</script>

<div class="age-dist-plot">
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
				x=-{marginLeft - 30}
				y={marginTop - 10}
				dy="0.32em"
				fill="#000"
				font-weight=800
				font-size=20
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
				font-weight=800
				font-size=20
				text-anchor="start"
			>
				Number of People
			</text>
		</g>
		<g 
		class='bar'
		>
			{#each stackedData as ages}
				{#each ages as d, i}
					<rect
						x={x(d[0]) + marginLeft}
						y={y(d.data.Degree_Abv) - 15}
						width={x(d[1] - d[0])}
						height={30}
						fill={color(ages.key)}
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
				x=1150
				y={marginTop - 20}
				width=160
				height=160
				fill={'#e7e6eb'}
				opacity=0.5
			/>
		</g>

		<g class='legend-dots'>
			{#each subgroups as key, i}
				<rect
					x=1160
					y={marginTop + (i * 30)}
					width=20
					height=20
					fill={color(key)}
				/>
			{/each}
		</g>

		<g class='legend-labels'>
			{#each labels as key, i}
				<text
					x={1190}
					y={marginTop + (i * 30) + 15}
					width=20
					height=20
					font-weight=400
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
			<b>{ageData[hovered].Degree}</b>
			<br>
			number of 25-34 y/o: {ageData[hovered][25]} ({(ageData[hovered][25] * 100.0 / ageData[hovered].Total).toFixed(1)}%)
			<br>
			number of 35-44 y/o: {ageData[hovered][35]} ({(ageData[hovered][35] * 100.0 / ageData[hovered].Total).toFixed(1)}%)
			<br>
			number of 45-54 y/o: {ageData[hovered][45]} ({(ageData[hovered][45] * 100.0 / ageData[hovered].Total).toFixed(1)}%)
			<br>
			number of 55-64 y/o: {ageData[hovered][55]} ({(ageData[hovered][55] * 100.0 / ageData[hovered].Total).toFixed(1)}%)
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
		width: 400px;
		color: 35-44;
		position: absolute;
		text-align: left;
		padding: 10px;
	}

	.y-axis {
		font: 12px sans-serif;
		font-family: "Assistant", sans-serif;
		font-weight: 600;
	}

	.x-axis {
		font: 14px sans-serif;
		font-family: "Assistant", sans-serif;
		font-weight: 600;
	}
</style>
