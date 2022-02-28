<script>
	import { onDestroy } from 'svelte';
    import mapbox from 'mapbox-gl';
    import { onMount } from "svelte";

    mapbox.accessToken = 'pk.eyJ1IjoiZG9jazQyNDIiLCJhIjoiY2pjazE5eTM2NDl2aDJ3cDUyeDlsb292NiJ9.Jr__XbmAolbLyzPDj7-8kQ';
	// set the context here...

	export let lat;
	export let lon;
	export let zoom;


    onMount(async () => {
        load();
    });

	let container;
	let map;

	function load() {
		map = new mapbox.Map({
			container,
			style: 'mapbox://styles/dock4242/ckx9gpmev2qe114rzfbqsztst',
			center: [lon, lat],
			zoom,
		});
	}

    function handleClick() {
		map.setLayoutProperty(
            "acis-alltime",
            'visibility',
            'none'
        );

        map.setLayoutProperty(
            "acis-alltime-2",
            'visibility',
            'none'
        );

        map.setLayoutProperty(
            "acis-alltime-3",
            'visibility',
            'none'
        );
	}

	onDestroy(() => {
		if (map) map.remove();
	});
</script>

<!-- this special element will be explained in a later section -->
<svelte:head>
	<link
		rel="stylesheet"
		href="https://unpkg.com/mapbox-gl/dist/mapbox-gl.css"
		on:load={load}
	/>
</svelte:head>

<button on:click={handleClick}>test</button>
<div bind:this={container}>
	{#if map}
		<slot />
	{/if}
</div>

<style>
	div {
		width: 100%;
		height: 700px;
	}
</style>
