<script>
    const parallaxConfig = [
        {name: "sky.png", offset: [0,0], speed: 0.99},
        {name: "far_mountains.png", offset: [0,0], speed: 0.95},
        {name: "near_mountains.png", offset: [0,0], speed: 0.9},
        {name: "trees.png", offset: [0,0], speed: 0.8},
        {name: "wtf.png", offset: [0,0], speed: 0.7},
        {name: "barn.png", offset: [0,0], speed: 0.6},
        {name: "far_field.png", offset: [0,0], speed: 0.5},
        {name: "near_field.png", offset: [0,0], speed: 0.4},
        {name: "cocks.png", offset: [0,0], speed: 0.3},
        {name: "goldy.png", offset: [0,0], speed: 0.2},
        {name: "hen.png", offset: [0,0], speed: 0.1},
        {name: "bottom.png", offset: [0,220], speed: -0.2},
        {name: "aw_my_gawd.png", offset: [0,0], speed: 0.0},
    ];

    let width = $state(1920);
    let height = $state(1080);
    let scroll = $state(0);

    let configWidth = 2696;
    let configHeight = 1418;
    let configAspectRatio = $derived(configWidth / configHeight);

    let parallaxHeight = $derived(height);
    let parallaxWidth = $derived(parallaxHeight * configAspectRatio);
</script>

<svelte:window bind:outerWidth={width} bind:outerHeight={height} bind:scrollY={scroll}></svelte:window>

<main class="main">
    <div class="parallax">
        {#each parallaxConfig as c, i}
        <div 
            class="parallax-layer"
            style:background-image="url('./images/{c.name}')"
            style:background-size="{parallaxWidth}px {parallaxHeight}px"
            style:top="{c.offset[1] + scroll * c.speed}px"
        ></div>
        {/each}
    </div>
    <div class="info">
        info
    </div>
</main>

<style>
.main {
    display: flex;
    align-content: center;
    justify-content: center;
    flex-direction: column;
}

.parallax {
    width: 100%;
    height: 100vh;
    overflow: hidden;
    position: relative;
}

.parallax-layer {
    width: 100%;
    height: 100%;
    position: absolute;
    background-position: center center;
}

.info {
    background-color: gray;
    width: 100%;
    min-height: 100vh;
}
</style>