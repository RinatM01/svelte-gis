<script>
	import * as d3 from 'd3';
	import distance from '../data/distance';
	export let width = 1000;
	export let height = 550;
	export let marginTop = 50;
	export let marginRight = 10;
	export let marginBottom = 75;
	export let marginLeft = 40;
	const labels = distance.map((x) => x.range);

	let isOver = false;
	let rangeName = '';
	let perc = 0;
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

	let mouseOver = function (e, range, area) {
		isOver = true;
		rangeName = range;
		perc = area;
		let toolTip = d3.select('#Tooltip-Distance');
		let toolTip2 = document.querySelector('#Tooltip-Distance');
		let bod = document.querySelector('#Container-Distance');
		// m.x = event.clientX;
		// m.y = event.clientY;
		toolTip
			.style('opacity', 1)
			.style(
				'left',
				// @ts-ignore
				`${e.x - bod.offsetLeft - toolTip2.offsetWidth / 2}px`
			)
			// @ts-ignore
			.style('top', `${yScale(area) - toolTip2.offsetHeight - 10}px`);
		// console.log(d.target);
		d3.selectAll('.Bar-d')
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
		d3.select('#Tooltip-Distance').style('opacity', 0);
		d3.selectAll('.Bar-d')
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
	id="Container-Distance"
	class="relative"
>
	<div
		id="Tooltip-Distance"
		class={`p-2 bg-slate-100 rounded-lg absolute pointer-events-none opacity-0`}
	>
		<h3 class="m-0 p-0 text-lg">{rangeName}</h3>
		<p>{perc}%</p>
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
			{#each distance as dist}
				<rect
					x={xScale(dist.range)}
					y={yScale(dist.lost)}
					height={yScale(0) - yScale(dist.lost)}
					width={xScale.bandwidth() / 2}
					fill="steelblue"
					class="Bar-d cursor-pointer"
					on:mouseover={(e) => {
						mouseOver(e, dist.range, dist.lost);
					}}
					on:mouseout={mouseLeave}
				/>
			{/each}
			{#each distance as dist}
				<rect
					x={xScale(dist.range) + xScale.bandwidth() / 2}
					y={yScale(dist.gained)}
					height={yScale(0) - yScale(dist.gained)}
					width={xScale.bandwidth() / 2}
					fill="orange"
					class="Bar-d cursor-pointer"
					on:mouseover={(e) => {
						mouseOver(e, dist.range, dist.gained);
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

			{#each distance as dist}
				<!-- X-Axis Ticks -->
				<line
					stroke="currentColor"
					x1={xScale(dist.range) + xScale.bandwidth() / 2}
					x2={xScale(dist.range) + xScale.bandwidth() / 2}
					y1={0}
					y2={6}
				/>

				<!-- X-Axis Tick Labels -->
				<text
					fill="currentColor"
					text-anchor="start"
					x={xScale(dist.range) + xScale.bandwidth() / 6}
					y={22}
				>
					{dist.range}
				</text>
			{/each}

			<!-- X-Axis Label -->
			<text
				fill="currentColor"
				x={width / 2 - 70}
				y={60}
				font-weight="bold"
				font-size="18">Distance from the center of the city</text
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
				Perentage of Total Area (%)
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
