<script>
	import { onMount } from 'svelte';

	const moo_start = [-10, 20];
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
		let pageWidth = window.innerWidth;
		let pageHeight = window.innerHeight;
		let landscape = pageWidth > pageHeight;

		let imageHeight = 1300;
		let imageWidth = 1584;
		let imageAspectRatio = imageWidth / imageHeight;

		let fuckme = pageWidth > imageWidth;

		let finalWidth = 0;
		let finalHeight = 0;
		if (fuckme) {
			finalWidth = pageWidth;
			finalHeight = pageWidth / imageAspectRatio;
		} else {
			finalWidth = imageWidth / imageAspectRatio;
			finalHeight = pageHeight;
		}

		function init() {
			let i = 0;
			for (let [element_id, properties] of Object.entries(config)) {
				var element = document.getElementById(element_id);
				element.style.backgroundImage = `url("./images/${element_id}"`;
				element.style.backgroundSize = `${finalWidth}px ${finalHeight}px `;
			}

			draw(0);
		}

		init();

		let lastScroll = 0;
		let ticking = false;

		function draw(scroll) {
			config['aw_shit.png'].visible = scroll / finalHeight > 0.5;

			console.log(scroll);

			document.getElementById('title').style.marginTop = `calc(5vh + ${scroll * 0.6}px`;
			document.getElementById('parallax').style.height = `${finalHeight - 200}px`;

			let my_planet_needs_me = scroll / pageHeight;

			config['moo.png'].offset = Lerp2D(moo_start, moo_end, my_planet_needs_me);

			for (let [element_id, properties] of Object.entries(config)) {
				var element = document.getElementById(element_id);
				element.style.visibility = properties.visible ? 'visible' : 'hidden';
				element.style.marginLeft = `${properties.offset[0]}px`;
				element.style.marginTop = `calc(${Math.max(
					properties.offset[1] / imageAspectRatio + scroll * properties.speed,
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
	<div id="parallax" class="parallax">
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

	<div id="info" class="info"></div>
</main>

<style>
	.main {
		display: flex;
		align-content: center;
		justify-content: center;
		flex-direction: column;
		background-color: rgb(173, 235, 243);
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
		background-color: rgb(68, 51, 27);
		overflow: hidden;
		width: 100%;
		position: relative;
		display: grid;
		grid-template-areas: 'bla';
		height: 1300px;
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
	}

	@media (min-width: 1920px) {
		.parallax-layer {
			background-position-x: center;
		}

		.title {
			font-size: 5rem;
		}
	}
	@media (max-width: 1920px) {
		.parallax-layer {
			background-position-x: center;
		}

		.title {
			font-size: 5rem;
		}
	}
	@media (max-width: 1280px) {
		.parallax-layer {
			background-position-x: 35%;
		}

		.title {
			font-size: 3rem;
		}
	}
	@media (max-width: 480px) {
		.parallax-layer {
			background-position-x: 27.5%;
		}

		.title {
			font-size: 3rem;
		}
	}
</style>
