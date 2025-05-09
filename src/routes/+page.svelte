<script lang="ts">
	import { onMount } from 'svelte';

	let width = $state(1920);
	let height = $state(1080);
	let scroll = $state(0);

	onMount(() => {
		setTimeout(() => {
			requestAnimationFrame(() => {
				window.scrollTo({
					top: window.innerHeight * 0.2,
					behavior: 'smooth'
				});
			});
		}, 1000);
	});

	const configWidth = 1920;
	const configHeight = 1262;
	const configAspectRatio = $derived(configWidth / configHeight);

	const landscape = $derived(width > height);

	const parallaxHeight = $derived(landscape ? width / configAspectRatio : height);
	const parallaxWidth = $derived(landscape ? width : height * configAspectRatio);

	const aw_shit = $derived(scroll / height > 0.8);

	const moo_start: [number, number] = $derived([-5, 20]);
	const moo_end: [number, number] = $derived([65, -130]);
	const my_planet_needs_me = $derived(scroll / height);

	function Lerp(from: number, to: number, t: number): number {
		return (1 - t) * from + t * to;
	}

	function Lerp2D(from: [number, number], to: [number, number], t: number): [number, number] {
		return [Lerp(from[0], to[0], t), Lerp(from[1], to[1], t)];
	}

	const parallaxConfig = {
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

	$effect(() => {
		parallaxConfig['aw_shit.png'].visible = aw_shit;
		parallaxConfig['moo.png'].offset = Lerp2D(moo_start, moo_end, my_planet_needs_me);
		// parallaxConfig['moo.png'].scale = Lerp(1.0, 0.1, my_planet_needs_me);
	});
</script>

<svelte:window bind:outerWidth={width} bind:outerHeight={height} bind:scrollY={scroll} />

<main class="main">
	<div class="parallax">
		<div
			class="sky"
			style:background-image="url('./images/sky.png')"
			style:background-size="{parallaxWidth}px {parallaxHeight}px"
			style:margin-top="calc({scroll * 1.0}px)"
		></div>

		<img
			class="title"
			src="./images/logo.webp"
			alt="depicts the official GoldenHorn logo of a black goat with long curved golden horns"
			style:margin-top="calc(20vh + {scroll * 0.6}px)"
		/>

		{#each Object.entries(parallaxConfig) as [name, c], i}
			<div
				class="parallax-layer"
				style:visibility={c.visible ? 'visible' : 'hidden'}
				style:background-image="url('./images/{name}')"
				style:background-size="{parallaxWidth}px {parallaxHeight}px"
				style:margin-left="{c.offset[0]}px"
				style:margin-top="calc(var(--sky-height) + {Math.max(
					c.offset[1] * configAspectRatio + scroll * c.speed,
					c.max[1]
				)}px)"
				style:transform="translateY(100px)"
				style:z-index={i}
			></div>
		{/each}
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
	}

	.title {
		grid-area: bla;
		display: flex;
		position: sticky;
		align-self: start;
        justify-self: center;
		width: 20rem;
		z-index: 15;
	}

	.parallax {
		height: 185vh;
		width: 100%;
		position: relative;
		overflow: hidden;
		display: grid;
		grid-template-areas: 'bla';
	}

	.sky {
		position: sticky;
		grid-area: bla;
		width: 100%;
		height: 100%;
		background-repeat: no-repeat;
		background-position-x: center;
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

	@media (min-width: 1921px) {
		.parallax {
			margin-bottom: -30vh;
		}

		.parallax-layer {
			background-position: center top;
		}

		.title {
			font-size: 5rem;
		}
	}
	@media (max-width: 1920px) {
		.parallax {
			margin-bottom: -30vh;
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
			margin-bottom: -50vh;
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
			margin-bottom: -50vh;
		}

		.parallax-layer {
			background-position-x: 27.5%;
		}

		.title {
			font-size: 3rem;
		}
	}
</style>
