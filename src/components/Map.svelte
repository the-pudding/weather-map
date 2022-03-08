<script>
	import { onDestroy } from 'svelte';
    import mapbox from 'mapbox-gl';
    import { onMount } from "svelte";
    import { timeFormat } from "d3";

    export let data;
    export let least;

    mapbox.accessToken = 'pk.eyJ1IjoiZG9jazQyNDIiLCJhIjoiY2pjazE5eTM2NDl2aDJ3cDUyeDlsb292NiJ9.Jr__XbmAolbLyzPDj7-8kQ';
	// set the context here...

	export let lat;
	export let lon;
	export let zoom;

    const formatTime = timeFormat("%B %d, %Y");
    const formatTimeNoYear = timeFormat("%B %d");


    let layers = [
        "daily","daily-days","monthly","alltime"
    ]

    let layersMin = [
        "daily-night","daily-days-night","monthly-night","alltime-night"
    ]

    onMount(async () => {
        load();

    });

	let container;
	let map;

    const bounds = [
        [-124.7844079, 24.7433195], // Southwest coordinates
        [ -66.9513812, 49.3457868],   // Northeast coordinates
        // [32.958984, -5.353521], // southwestern corner of the bounds
        // [43.50585, 5.615985] // northeastern corner of the bounds

    ];

	function load() {
		map = new mapbox.Map({
			container,
			style: 'mapbox://styles/dock4242/cl0banu3w000i14l8w1woe65r',
            // maxBounds: bounds,
			center: [-95.973515, 38.382024],//,
            zoom: 7
		});

        map.fitBounds(bounds, {padding: 0});

        map.on('load', function(){

            function sizeFont(days){
                if(days < 30){
                    return 20
                }
                if(days < 60){
                    return 16
                }
                return 14;
            }

            const matchExpression = ['match', ['get','station']];
            const sizeExpression = ['match', ['get','station']];

            for (const row of data){
                matchExpression.push(row['station'],row["days_since_daily"]);
                sizeExpression.push(row['station'],sizeFont(+row["days_since_daily"]));
            }

            matchExpression.push("red");
            sizeExpression.push(12);


            const markerHeight = 17;
            const markerRadius = 0;
            const linearOffset = 0;
            const popupOffsets = {
                'top': [0, 0],
                'top-left': [0, 0],
                'top-right': [0, 0],
                'bottom': [0, -markerHeight],
                'bottom-left': [linearOffset, (markerHeight - markerRadius + linearOffset) * -1],
                'bottom-right': [-linearOffset, (markerHeight - markerRadius + linearOffset) * -1],
                'left': [markerRadius, (markerHeight - markerRadius) * -1],
                'right': [-markerRadius, (markerHeight - markerRadius) * -1]
            };



            const stationsMid = data.filter(function(d){
                return +d["days_since_daily"] < 30 && d["station"] != least.station;
            }).map(d => d.station);

            const stationsBase = data.filter(function(d){
                return +d["days_since_daily"] > 29 && d["station"] != least.station;
            }).map(d => d.station);



            map.setLayoutProperty('acis-agg-2-986vec-top-layer', 'text-field', matchExpression);
            map.setLayoutProperty('acis-agg-2-986vec-mid-layer', 'text-field', matchExpression);
            map.setLayoutProperty('acis-agg-2-986vec', 'text-field', matchExpression);


            console.log(sizeExpression)

            map.setLayoutProperty('acis-agg-2-986vec-mid-layer', 'text-size', sizeExpression);
            map.setLayoutProperty('acis-agg-2-986vec', 'text-size', sizeExpression);



            map.setFilter('acis-agg-2-986vec-top-layer', [ "all", [ "match", ["get", "station"], [least.station], true, false ] ]);
            map.setFilter('acis-agg-2-986vec-mid-layer', [ "all", [ "match", ["get", "station"], stationsMid, true, false ] ]);
            map.setFilter('acis-agg-2-986vec', [ "all", [ "match", ["get", "station"], stationsBase, true, false ] ]);



            let leastDate = new Date(least.days_since_daily_date.slice(0,4), +least.days_since_daily_date.slice(5,7) -1, least.days_since_daily_date.slice(8,10));

            new mapbox.Popup({ offset: popupOffsets, focusAfterOpen: false, maxWidth: '340px' })
                .setLngLat([least.longitude,least.latitude])
                //.setHTML("hi")
                .setHTML(`${least.days_since_daily} days ago, it was ${least.days_since_val}°F in <b>${least.name}, ${least.state_abbr}</b> on <b>${formatTime(leastDate)}</b>, the hottest ${formatTimeNoYear(leastDate)} on record for the city.`)
                .addTo(map);


        })

	}

    function handleClick(mapLayer) {

        let toHide = layers.filter(d => d != mapLayer);
        toHide = toHide.concat(layersMin.filter(d => d != mapLayer));

        for (let layer in toHide){
        
            map.setLayoutProperty(
                `acis-${toHide[layer]}`,
                'visibility',
                'none'
            );

            map.setLayoutProperty(
                `acis-${toHide[layer]}-2`,
                'visibility',
                'none'
            );

            map.setLayoutProperty(
                `acis-${toHide[layer]}-3`,
                'visibility',
                'none'
            );
            
        }

		map.setLayoutProperty(
            `acis-${mapLayer}`,
            'visibility',
            'visible'
        );

        map.setLayoutProperty(
            `acis-${mapLayer}-2`,
            'visibility',
            'visible'
        );

        map.setLayoutProperty(
            `acis-${mapLayer}-3`,
            'visibility',
            'visible'
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
<!-- 
<p>Day-time Max</p>
{#each layers as layer}
    <button on:click={() => handleClick(layer)}>{layer}</button>
{/each}

<p>Night-time Max</p>
{#each layersMin as layer}
    <button on:click={() => handleClick(layer)}>{layer}</button>
{/each} -->


<div class="mapbox-container" bind:this={container}>
	{#if map}
		<slot />
	{/if}
</div>

<style>
	.mapbox-container {
		width: 100%;
		height: 75vw;
	}
</style>
