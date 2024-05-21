<script>
	export let dimensions = {
		margin: {
			left: 0,
			right: 0,
			top: 0,
			bottom: 0,
		},
	};
	export let colors = [];
	export let categories = [];
</script>

<g
	transform={`translate(${dimensions.width - dimensions.offset.x}, ${dimensions.height - dimensions.offset.y})`}
>
	<text
		font-size="18"
		font-weight="bold"
		y="-8"
		>Percentage of Green Area Lost
	</text>
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
