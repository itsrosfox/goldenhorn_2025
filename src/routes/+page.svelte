<script lang="ts">
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';

	let width = $state(1920);
	let height = $state(1080);
	let scroll = $state(0);

	// onMount(() => {
	// 	// give the browser a tick to render
	// 	requestAnimationFrame(() => {
	// 		window.scrollTo({
	// 			top: window.innerHeight * 0.2, // for example, 80% down
	// 			behavior: 'smooth'
	// 		});
	// 	});
	// });

	const configWidth = 2696;
	const configHeight = 1418;
	const configAspectRatio = $derived(configWidth / configHeight);

	const parallaxHeight = $derived(height);
	const parallaxWidth = $derived(parallaxHeight * configAspectRatio);

	const aw_shit = $derived(scroll / height > 0.6);

	const moo_start: [number, number] = $derived([-10, 20]);
	const moo_end: [number, number] = $derived([80, -100]);
	const my_planet_needs_me = $derived(scroll / height);

	function Lerp(from: number, to: number, t: number): number {
		return from + t * (to - from);
	}

	function Lerp2D(from: [number, number], to: [number, number], t: number): [number, number] {
		return [Lerp(from[0], to[0], t), Lerp(from[1], to[1], t)];
	}

	/*style:top="{Math.max(c.offset[1] * configAspectRatio + scroll * c.speed, 0)}px"*/

	const parallaxConfig = {
		'sky.png': { offset: [0, 0], max: [0, 0], speed: 0.99, visible: true, scale: 1.0 },
		'far_mountains.png': { offset: [0, 0], max: [0, 0], speed: 0.95, visible: true, scale: 1.0 },
		'near_mountains.png': { offset: [0, 0], max: [0, 0], speed: 0.9, visible: true, scale: 1.0 },
		'trees.png': { offset: [0, 0], max: [0, 0], speed: 0.8, visible: true, scale: 1.0 },
		'moo.png': { offset: moo_start, max: [0, 0], speed: 0.7, visible: true, scale: 1.0 },
		'wtf.png': { offset: [0, 0], max: [0, 0], speed: 0.7, visible: true, scale: 1.0 },
		'barn.png': { offset: [0, 0], max: [0, 0], speed: 0.6, visible: true, scale: 1.0 },
		'far_field.png': { offset: [0, 0], max: [0, 0], speed: 0.5, visible: true, scale: 1.0 },
		'near_field.png': { offset: [0, 0], max: [0, 0], speed: 0.4, visible: true, scale: 1.0 },
		'cocks.png': { offset: [0, 0], max: [0, 0], speed: 0.3, visible: true, scale: 1.0 },
		'goldy.png': { offset: [0, 0], max: [0, 0], speed: 0.2, visible: true, scale: 1.0 },
		'hen.png': { offset: [0, 0], max: [0, 0], speed: 0.1, visible: true, scale: 1.0 },
		'aw_shit.png': { offset: [0, 0], max: [0, 0], speed: 0.1, visible: false, scale: 1.0 },
		'bottom.png': { offset: [0, 200], max: [0, 50], speed: -0.4, visible: true, scale: 1.0 },
		'aw_my_gawd.png': { offset: [0, 0], max: [0, 0], speed: 0.0, visible: true, scale: 1.0 }
	};

	$effect(() => {
		parallaxConfig['aw_shit.png'].visible = aw_shit;
		parallaxConfig['moo.png'].offset = Lerp2D(moo_start, moo_end, my_planet_needs_me);
	});
</script>

<svelte:window bind:outerWidth={width} bind:outerHeight={height} bind:scrollY={scroll} />

<main class="main">
	<div class="title" style:top="calc(25vh + {scroll * 0.5}px)">GoldenHorn '25</div>
	<div class="parallax">
		{#each Object.entries(parallaxConfig) as [name, c], i}
			<div
				class="parallax-layer"
				style:visibility={c.visible ? 'visible' : 'hidden'}
				style:background-image="url('./images/{name}')"
				style:background-size="{parallaxWidth}px {parallaxHeight}px"
				style:left="{c.offset[0]}px"
				style:top="calc(var(--sky-height) + {Math.max(
					c.offset[1] * configAspectRatio + scroll * c.speed,
					c.max[1]
				)}px)"
				style:scale={c.scale}
			></div>
		{/each}
	</div>
	<div class="info">info</div>
</main>

<style>
	:root {
		--sky-height: 40vh;
	}

	.main {
		display: flex;
		align-content: center;
		justify-content: center;
		flex-direction: column;
		background-color: rgb(160, 233, 241);
	}

	.title {
		top: var(--sky-height);
		display: flex;
		position: absolute;
		width: 100%;
		align-items: center;
		justify-content: center;
		z-index: 1;
	}

	.parallax {
		width: 100%;
		height: 140vh;
		overflow: hidden;
		position: relative;
	}

	.parallax-layer {
		width: 100%;
		height: 100%;
		position: absolute;
		background-repeat: no-repeat;
	}

	.info {
		background-color: rgb(68, 51, 27);
		width: 100%;
		min-height: 100vh;
	}

	@media (min-width: 1921px) {
		.parallax-layer {
			background-position: center top;
		}

		.title {
			font-size: 5rem;
		}
	}
	@media (max-width: 1920px) {
		.parallax-layer {
			background-position: center top;
		}

		.title {
			font-size: 5rem;
		}
	}
	@media (max-width: 1280px) {
		.parallax-layer {
			background-position: 35% center;
		}

		.title {
			font-size: 3rem;
		}
	}
	@media (max-width: 480px) {
		.parallax-layer {
			background-position: 30% center;
		}

		.title {
			font-size: 3rem;
		}
	}
</style>
