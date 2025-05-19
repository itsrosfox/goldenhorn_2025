<script>
	import { onMount } from 'svelte';

	const moo_start = [-10,20];
	const moo_end = [50, -200];

	function Lerp(from, to, t) {
		return (1 - t) * from + t * to;
	}

	function Lerp2D(from, to, t) {
		return [Lerp(from[0], to[0], t), Lerp(from[1], to[1], t)];
	}

	const config = {
		'sky.png': { offset: [0, 0], max: [0, 0], speed: 1.0, visible: true, scale: 1.0 },
		'far_mountains.png': { offset: [0, 0], max: [0, 0], speed: 0.93, visible: true, scale: 1.0 },
		'near_mountains.png': { offset: [0, 0], max: [0, 0], speed: 0.9, visible: true, scale: 1.0 },
		'trees.png': { offset: [0, 0], max: [0, 0], speed: 0.8, visible: true, scale: 1.0 },
		'moo.png': { offset: moo_start, max: [0, 0], speed: 0.7, visible: true, scale: 1.0 },
		'mukice.png': { offset: [0, 0], max: [0, 0], speed: 0.7, visible: true, scale: 1.0 },
		'ufo.png': { offset: [0, 0], max: [0, 0], speed: 0.7, visible: true, scale: 1.0 },
		'barn.png': { offset: [0, 0], max: [0, 0], speed: 0.6, visible: true, scale: 1.0 },
		'far_field.png': { offset: [0, 0], max: [0, 0], speed: 0.5, visible: true, scale: 1.0 },
		'near_field.png': { offset: [0, 0], max: [0, 0], speed: 0.4, visible: true, scale: 1.0 },
		'cocks.png': { offset: [0, 0], max: [0, 0], speed: 0.3, visible: true, scale: 1.0 },
		'goldy.png': { offset: [0, 0], max: [0, 0], speed: 0.2, visible: true, scale: 1.0 },
		'hen.png': { offset: [0, 0], max: [0, 0], speed: 0.1, visible: true, scale: 1.0 },
		'aw_shit.png': { offset: [0, 0], max: [0, 0], speed: 0.1, visible: false, scale: 1.0 },
		'bottom.png': { offset: [0, 350], max: [0, 150], speed: -0.4, visible: true, scale: 1.0 },
		'aw_my_gawd.png': { offset: [0, 0], max: [0, 0], speed: 0.1, visible: true, scale: 1.0 },
		'leaves.png': { offset: [0, 0], max: [0, 0], speed: 0.1, visible: true, scale: 1.0 }
	};

	onMount(() => {
		let width = window.innerWidth;
		let height = window.innerHeight;
		let landscape = width > height;
		let aspectRatio = 1584 / 1300;

		function init() {
			let i = 0;
			for (let [element_id, properties] of Object.entries(config)) {
				var element = document.getElementById(element_id);
				element.style.backgroundImage = `url("./images/${element_id}"`;
				element.style.backgroundSize = `
			${landscape ? width : height * aspectRatio}px	
			${landscape ? width / aspectRatio : height}px `;
				element.style.zIndex = i++;
			}

			draw(0);
		}

		init();

		let lastScroll = 0;
		let ticking = false;

		function draw(scroll) {
			config['aw_shit.png'].visible = scroll / height > 0.8;

			document.getElementById('title').style.marginTop = `calc(20vh + ${scroll * 0.6}px`;

			let my_planet_needs_me = scroll / height;

			config['moo.png'].offset = Lerp2D(moo_start, moo_end, my_planet_needs_me);

			for (let [element_id, properties] of Object.entries(config)) {
				var element = document.getElementById(element_id);
				element.style.visibility = properties.visible ? 'visible' : 'hidden';
				element.style.marginLeft = `${properties.offset[0]}px`;
				element.style.marginTop = `calc(var(--sky-height) + ${Math.max(
					properties.offset[1] / aspectRatio + scroll * properties.speed,
					properties.max[1]
				)}px`;
			}
		}

		document.addEventListener('scroll', (event) => {
			lastScroll = window.scrollY;

			if (!ticking) {
				window.requestAnimationFrame(() => {
					draw(lastScroll);
					ticking = false;
				});

				ticking = true;
			}
		});
	});
</script>

<svelte:window />

<main class="main">
	<div class="parallax">
		<img
			id="title"
			class="title"
			src="./images/logo.webp"
			alt="depicts the official GoldenHorn logo of a black goat with long curved golden horns"
		/>
		<div id="sky.png" class="parallax-layer"></div>
		<div id="far_mountains.png" class="parallax-layer"></div>
		<div id="near_mountains.png" class="parallax-layer"></div>
		<div id="trees.png" class="parallax-layer"></div>
		<div id="moo.png" class="parallax-layer"></div>
		<div id="mukice.png" class="parallax-layer"></div>
		<div id="ufo.png" class="parallax-layer"></div>
		<div id="barn.png" class="parallax-layer"></div>
		<div id="far_field.png" class="parallax-layer"></div>
		<div id="near_field.png" class="parallax-layer"></div>
		<div id="cocks.png" class="parallax-layer"></div>
		<div id="goldy.png" class="parallax-layer"></div>
		<div id="hen.png" class="parallax-layer"></div>
		<div id="aw_shit.png" class="parallax-layer"></div>
		<div id="bottom.png" class="parallax-layer"></div>
		<div id="aw_my_gawd.png" class="parallax-layer"></div>
		<div id="leaves.png" class="parallax-layer"></div>
	</div>

	<div class="info"></div>
</main>

<style>
	:root {
		--sky-height: 30vh;
	}

	.main {
		display: flex;
		align-content: center;
		justify-content: center;
		flex-direction: column;
		background-color: rgb(173, 235, 243);
		overflow: hidden;
	}

	.title {
		grid-area: bla;
		display: flex;
		position: sticky;
		align-self: start;
		justify-self: center;
		width: 20rem;
		z-index: 5;
	}

	.parallax {
		height: 185vh;
		width: 100%;
		position: relative;
		display: grid;
		grid-template-areas: 'bla';
	}

	.parallax-layer {
		grid-area: bla;
		width: 100%;
		height: 100%;
		position: sticky;
		background-repeat: no-repeat;
	}

	.info {
		background-color: rgb(68, 51, 27);
		width: 100%;
		min-height: 100vh;
		z-index: 10;
	}

	@media (min-width: 1920px) {
		.parallax {
			margin-bottom: calc(-30vh);
		}

		.parallax-layer {
			background-position-x: center;
		}

		.title {
			font-size: 5rem;
		}
	}
	@media (max-width: 1920px) {
		.parallax {
			margin-bottom: calc(-50vh);
		}

		.parallax-layer {
			background-position-x: center;
		}

		.title {
			font-size: 5rem;
		}
	}
	@media (max-width: 1280px) {
		.parallax {
			margin-bottom: calc(-65vh);
		}

		.parallax-layer {
			background-position-x: 35%;
		}

		.title {
			font-size: 3rem;
		}
	}
	@media (max-width: 480px) {
		.parallax {
			margin-bottom: calc(-65vh);
		}

		.parallax-layer {
			background-position-x: 27.5%;
		}

		.title {
			font-size: 3rem;
		}
	}
</style>
