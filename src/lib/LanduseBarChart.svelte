<script>
	import * as d3 from 'd3';
	import landuse from '../data/landuse';
	// The chart dimensions and margins as optional props.
	export let width = 1000;
	export let height = 600;
	export let marginTop = 50;
	export let marginRight = 10;
	export let marginBottom = 150;
	export let marginLeft = 40;
	const labels = landuse.map((x) => x.type);

	let isOver = false;
	let landuseType = '';
	let areaNum = 0;
	// Create the x (horizontal position) scale.
	const xScale = d3
		.scaleBand()
		.domain(labels)
		.range([marginLeft, width - marginRight])
		.padding(0.2);

	// Create the y (vertical position) scale.
	const yScale = d3
		.scaleLinear()
		.domain([0, 8])
		.range([height - marginBottom, marginTop]);

	let mouseOver = function (e, type, area) {
		isOver = true;
		landuseType = type;
		areaNum = area;
		let toolTip = d3.select('#Tooltip-Landuse');
		let toolTip2 = document.querySelector('#Tooltip-Landuse');
		let bod = document.querySelector('#Container-Landuse');
		// m.x = event.clientX;
		// m.y = event.clientY;
		toolTip
			.style('opacity', 1)
			.style(
				'left',
				// @ts-ignore
				`${e.x - bod.offsetLeft - toolTip2.offsetWidth / 2 - xScale.bandwidth() / 4}px`
			)
			// @ts-ignore
			.style('top', `${yScale(area) - toolTip2.offsetHeight - 10}px`);
		// console.log(d.target);
		d3.selectAll('.Bar')
			.transition()
			.duration(100)
			.style('opacity', 0.2);
		d3.select(e.target)
			.transition()
			.duration(100)
			.style('opacity', 1);
		//.style('stroke', 'black');
	};

	let mouseLeave = function (d) {
		isOver = false;
		d3.select('#Tooltip-Landuse').style('opacity', 0);
		d3.selectAll('.Bar')
			.transition()
			.duration(100)
			.style('opacity', 1);
		d3.select(this).transition().duration(200);
		//.style('stroke', 'transparent');
	};
	let colors = ['steelblue', 'orange'];
	let categories = ['Green Area Lost', 'Green Area Gained'];
</script>

<div
	id="Container-Landuse"
	class="relative"
>
	<div
		id="Tooltip-Landuse"
		class={`p-2 bg-slate-100 rounded-lg absolute pointer-events-none opacity-0`}
	>
		<h3 class="m-0 p-0 text-lg">{landuseType}</h3>
		<p>{Math.floor(10 ** areaNum)}m<sup>2</sup></p>
	</div>
	<svg
		{width}
		{height}
		viewBox="0 0 {width} {height}"
		style:max-width="100%"
		style:height="auto"
	>
		<!-- Add a rect for each bar. -->
		<g>
			{#each landuse as land}
				<rect
					x={xScale(land.type)}
					y={yScale(land.lost)}
					height={yScale(0) - yScale(land.lost)}
					width={xScale.bandwidth() / 2}
					fill="steelblue"
					class="Bar cursor-pointer"
					on:mouseover={(e) => {
						mouseOver(e, land.type, land.lost);
					}}
					on:mouseout={mouseLeave}
				/>
			{/each}
			{#each landuse as land}
				<rect
					x={xScale(land.type) + xScale.bandwidth() / 2}
					y={yScale(land.gained)}
					height={yScale(0) - yScale(land.gained)}
					width={xScale.bandwidth() / 2}
					fill="orange"
					class="Bar cursor-pointer"
					on:mouseover={(e) => {
						mouseOver(e, land.type, land.gained);
					}}
					on:mouseout={mouseLeave}
				/>
			{/each}
		</g>

		<!-- X-Axis -->
		<g transform="translate(0,{height - marginBottom})">
			<line
				stroke="currentColor"
				x1={marginLeft - 6}
				x2={width}
			/>

			{#each landuse as land}
				<!-- X-Axis Ticks -->
				<line
					stroke="currentColor"
					x1={xScale(land.type) + xScale.bandwidth() / 2}
					x2={xScale(land.type) + xScale.bandwidth() / 2}
					y1={0}
					y2={6}
				/>

				<!-- X-Axis Tick Labels -->
				<text
					fill="currentColor"
					text-anchor="start"
					x={xScale(land.type) + xScale.bandwidth() / 2}
					y={22}
					transform={`rotate(40,${xScale(land.type) + xScale.bandwidth() / 2},${10})`}
				>
					{land.type}
				</text>
			{/each}

			<!-- X-Axis Label -->
			<text
				fill="currentColor"
				x={width / 2}
				y={130}
				font-weight="bold"
				font-size="18">Landuse Type</text
			>
		</g>

		<!-- Y-Axis -->
		<g transform="translate({marginLeft},0)">
			{#each yScale.ticks() as tick}
				<!-- 
        Y-Axis Ticks. 
        Note: First tick is skipped since the x-axis already acts as a tick. 
      -->
				{#if tick !== 0}
					<line
						stroke="currentColor"
						x1={0}
						x2={-6}
						y1={yScale(tick)}
						y2={yScale(tick)}
					/>
				{/if}

				<!-- Y-Axis Tick Labels -->
				<text
					fill="currentColor"
					text-anchor="end"
					dominant-baseline="middle"
					x={-9}
					y={yScale(tick)}
				>
					{Math.trunc(tick)}
				</text>
			{/each}

			<!-- Y-Axis Label -->
			<text
				fill="currentColor"
				text-anchor="start"
				x={-marginLeft}
				y={25}
				font-weight="bold"
				font-size="18"
			>
				Area ( log(m <tspan baseline-shift="super">2</tspan>) )
			</text>
		</g>
		<g transform={`translate(${width - 200}, ${110})`}>
			{#each colors as color, i}
				<g transform={`translate(0, ${i * 24})`}>
					<rect
						height="20"
						width="20"
						fill={color}
					/>
					{#if categories[i]}
						<text
							dominant-baseline="middle"
							y="14"
							x="28"
							font-size="14"
							font-weight="bold"
						>
							{categories[i]}
						</text>
					{/if}
				</g>
			{/each}
		</g>
	</svg>
</div>
