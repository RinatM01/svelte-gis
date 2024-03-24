<script>
	import * as d3 from 'd3';
	import { extent, rollup } from 'd3-array';
	import { csv, json } from 'd3-fetch';
	import { geoPath } from 'd3-geo';
	import { scaleQuantile, scaleLinear } from 'd3-scale';
	import data from '../data/tash.json';
	import Legend from './Legend.svelte';

	console.log(data);
	// Setting up the map
	let dimensions = {
		width: 975,
		height: 720,
		margin: {
			top: 24,
			right: 0,
			left: 0,
			bottom: 6,
		},
	};
	const currPorjection = d3
		.geoTransverseMercator()
		.rotate([-68, 0, 0])
		.fitSize([dimensions.width, dimensions.height], data);
	const path = geoPath(currPorjection);
	const tashFeatures = data.features;
	const ratios = data.features.map((f) => f.properties.GREEN_LOSS);
	let colors = [
		'rgb(247, 84, 84)',
		'rgb(242, 117, 117)',
		'rgb(243, 147, 147)',
		'rgb(241, 162, 162)',
	];
	let scale = scaleLinear()
		.domain([0.1, 0.08, 0.05, 0.03])
		.range(colors);
	const categories = ['15%-10%', '10%-8%', '8-5%', '5-3%'];

	let m = { x: 0, y: 0 };
	let isOver = false;
	let districtName = '';
	let districtLoss = 0;

	function handleMousemove(event) {
		let toolTip = d3.select('#Tooltip');
		let toolTip2 = document.querySelector('#Tooltip');
		// m.x = event.clientX;
		// m.y = event.clientY;
		toolTip
			.style('left', `${event.clientX}px`)
			// @ts-ignore
			.style('top', `${event.clientY - toolTip2.offsetHeight}px`);
	}

	let mouseOver = function (e, feature) {
		m.x = e.offsetX;
		m.y = e.offsetY;
		isOver = true;
		districtName = feature.properties.ADM2_EN;
		districtLoss = feature.properties.GREEN_LOSS;
		// console.log(d.target);
		d3.selectAll('.Country')
			.transition()
			.duration(200)
			.style('opacity', 0.2);
		d3.select(e.target)
			.transition()
			.duration(200)
			.style('opacity', 1);
		//.style('stroke', 'black');
	};

	let mouseLeave = function (d) {
		isOver = false;
		d3.selectAll('.Country')
			.transition()
			.duration(200)
			.style('opacity', 1);
		d3.select(this).transition().duration(200);
		//.style('stroke', 'transparent');
	};
</script>

<h1>Hiiii</h1>
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="flex"
	on:mousemove={handleMousemove}
>
	<div
		id="Tooltip"
		class={`Tooltip p-2 bg-slate-100 rounded-lg absolute pointer-events-none opacity-${isOver ? '1' : '0'}`}
	>
		<h3 class="m-0 p-0 text-lg">{districtName} District</h3>
		<p>{districtLoss * 100}%</p>
	</div>
	<svg
		width={dimensions.width}
		height={dimensions.height}
		viewBox={`0 0 ${dimensions.width} ${dimensions.height}`}
	>
		<g>
			<!-- <path fill="none" stroke="white" stroke-linejoin="round" d={path(stateMesh)} /> -->
			{#each tashFeatures as feature}
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<!-- svelte-ignore a11y-mouse-events-have-key-events -->
				<path
					fill={scale(feature.properties.GREEN_LOSS)}
					d={path(feature)}
					class={`Country`}
					on:mouseover={(e) => {
						mouseOver(e, feature);
					}}
					on:mouseout={mouseLeave}
				/>
			{/each}
		</g>
		<Legend
			{dimensions}
			{colors}
			{categories}
		/>
	</svg>
</div>

<style>
</style>
