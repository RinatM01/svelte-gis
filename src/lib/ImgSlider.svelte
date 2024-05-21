<script>
	const leftImg = 'tash2017.png';
	const leftAlt = 'Tashkent in 2017';
	const rightImg = 'tash2023.png';
	const rightAlt = 'Tashkent in 2023';

	let percent = 25;
	let x = 250;
	let bod = document.querySelector('#Container');
	console.log(bod);

	// let dragging = false;

	// function start() {
	// 	dragging = true;
	// }

	// function stop() {
	// 	dragging = false;
	// }

	// function mouseMove(e) {
	// 	if (dragging) {
	// 		let bod = document.querySelector('#Container');
	// 		x = e.x - e.srcElement.parentElement.offsetLeft;
	// 		//console.log(x);
	// 		percent =
	// 			((e.x - e.srcElement.parentElement.offsetLeft) /
	// 				bod.offsetWidth) *
	// 			100;
	// 	}
	// }
	function dragMe(node) {
		let moving = false;
		let left = 250;

		node.style.position = 'absolute';
		node.style.left = `${left}px`;
		node.style.cursor = 'move';
		node.style.userSelect = 'none';

		node.addEventListener('mousedown', () => {
			moving = true;
		});

		window.addEventListener('mousemove', (e) => {
			if (moving) {
				let bod = document.querySelector('#Container');
				//percent = ((e.x - bod.offsetLeft) / bod.offsetWidth) * 100;
				if (left > -1 && left < bod.offsetWidth + 1) {
					left += e.movementX;
				} else if (left <= 0) {
					left = 0.1;
				} else {
					left = bod.offsetWidth;
				}
				console.log(left);
				percent = (left / bod.offsetWidth) * 100;
				node.style.left = `${left}px`;
			}
		});

		window.addEventListener('mouseup', () => {
			moving = false;
		});
	}
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	id="Container"
	class="relative w-[450px] h-[320px] sm:w-[500px] sm:h-[355px] md:w-[1000px] md:h-[710.58px]"
>
	<img
		src={rightImg}
		alt={rightAlt}
		class="absolute w-[450px] sm:w-[500px] md:w-[1000px] h-auto non-draggable"
	/>

	<div
		use:dragMe
		class="absolute w-3 h-[320px] sm:h-[355px] md:h-[710.58px] top-0 bg-white z-10 cursor-pointer"
	>
		<div
			class="absolute top-[48%] left-[-1.4em] bg-white p-3 z-20 rounded-full cursor-pointer"
		>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="2em"
				height="2em"
				viewBox="0 0 14 14"
				{...$$props}
			>
				<path
					fill="currentColor"
					fill-rule="evenodd"
					d="M2.5.5a.5.5 0 0 0-.854-.354l-1.5 1.5a.5.5 0 0 0 0 .708l1.5 1.5A.5.5 0 0 0 2.5 3.5v-1h1a.5.5 0 0 0 0-1h-1zm2.407 3.299l.947 5.86l-.52.188a1.715 1.715 0 0 0-.658 2.799l1 1.045a1 1 0 0 0 .722.309h6.482a1 1 0 0 0 .994-1.113l-.367-3.236a2.573 2.573 0 0 0-2.951-2.13l-2.432.394l-.73-4.519a1.26 1.26 0 0 0-2.487.403m5.402-3.76a.5.5 0 0 1 .545.107l1.5 1.5a.5.5 0 0 1 0 .708l-1.5 1.5A.5.5 0 0 1 10 3.5v-1H9a.5.5 0 1 1 0-1h1v-1a.5.5 0 0 1 .309-.462Z"
					clip-rule="evenodd"
				/>
			</svg>
		</div>
	</div>

	<img
		src={leftImg}
		alt={leftAlt}
		class="absolute w-[450px] sm:w-[500px] w-[1000px] h-auto non-draggable"
		style={`clip-path: polygon(0% 0%, ${percent}% 0%, ${percent}% 100%, 0% 100%)`}
	/>
</div>

<style>
	.non-draggable {
		user-drag: none;
		-webkit-user-drag: none;
		user-select: none;
		-moz-user-select: none;
		-webkit-user-select: none;
		-ms-user-select: none;
	}
</style>
