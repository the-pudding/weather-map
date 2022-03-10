<script>
	import { onDestroy } from 'svelte';
    import mapbox from 'mapbox-gl';
    import { onMount } from "svelte";
    import { selectAll, timeFormat, geoAlbersUsa, geoMercator } from "d3";
    import viewport from "$stores/viewport.js";

    export let data;
    export let least;

    mapbox.accessToken = 'pk.eyJ1IjoiZG9jazQyNDIiLCJhIjoiY2pjazE5eTM2NDl2aDJ3cDUyeDlsb292NiJ9.Jr__XbmAolbLyzPDj7-8kQ';
	// set the context here...

	export let lat;
	export let lon;
	export let zoom;

    let mapWidth = 0;


    const formatTime = timeFormat("%B %d, %Y");
    const formatTimeNoYear = timeFormat("%B %d");
    let popupData = least;

    $: w = Math.min(mapWidth, $viewport.width) * 0.8;
    $: h = Math.min(mapWidth, $viewport.height) * 0.8;
    $: popupDataDate = new Date(popupData.days_since_daily_date.slice(0,4), +popupData.days_since_daily_date.slice(5,7) -1, popupData.days_since_daily_date.slice(8,10));

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
            [-22.275411,-13.090124],
            [20.68255,12.703398]
    ];

    let center = [-0.7964304999999996,-0.19336299999999973];

    let stationIds = data.map(function(d){
        return d.station;
    })


	function load() {

        let geojson = {
                "type":"FeatureCollection",
                "features":null
        };

        geojson.features = data.map(d => {
            return {
                "type":"Feature",
                "properties":{"station":d.station},
                "id":stationIds.indexOf(d.station),
                "geometry":{
                    "type":"Point",
                    "coordinates":[d.longitude,d.latitude]
                }
            }
        })

        let popup = null;

        if(!map){
            map = new mapbox.Map({
                container,
                style: 'mapbox://styles/dock4242/cl0bhzae7000y14utgxgy71f1', //'mapbox://styles/dock4242/cl0banu3w000i14l8w1woe65r',
                // // maxBounds: bounds,
                center: center,//[-95.973515, 38.382024],//,
                zoom: 7,
                scrollZoom: false
		    });

        }
		

        let padding = 0;
        let navPosition = 'top-right';
        if(w < 500) {
            padding = 0;
            navPosition = 'bottom-right';
        }

        

        let popupAdded = false;
        let hoveredStation = null;

        map.on('load', function(){
            
            map.fitBounds(bounds, {padding: padding});
            
            let sizeControlLength = selectAll(".mapboxgl-ctrl-zoom-in").size();
            
            if(sizeControlLength == 0){
                map.addControl(new mapbox.AttributionControl(), 'top-left');
                map.addControl(new mapbox.NavigationControl({visualizePitch:false, showCompass: false}), navPosition);
            }

            

            function sizeFont(days){

                if(w < 500){
                    if(days < 30){
                        return 16
                    }
                    if(days < 60){
                        return 14
                    }
                    return 11;
                }

                if(days < 30){
                    return 20
                }
                if(days < 60){
                    return 16
                }
                return 14;
            }

            let sources = map.getStyle().sources
            if(Object.keys(sources).indexOf("test-circles") == -1){
                map.addSource('test-circles', {
                    'type': 'geojson',
                    'data': geojson
                });

                map.addLayer({
                    'id': 'hover-effect',
                    'type': 'circle',
                    'source': 'test-circles',
                    'layout': {},
                    'paint': {
                        'circle-stroke-color': '#000',
                        'circle-radius': 13,
                        'circle-stroke-width': 2,
                        'circle-color': "rgba(0,0,0,0)",
                        'circle-stroke-opacity':[
                            'case',
                            ['boolean', ['feature-state', 'hover'], false],
                            1,
                            0
                        ]
                    }
                });
            }

            const matchExpression = ['match', ['get','station']];
            const sizeExpression = ['match', ['get','station']];

            for (const row of data){
                matchExpression.push(row['station'],row["days_since_daily"]);
                sizeExpression.push(row['station'],sizeFont(+row["days_since_daily"]));
            }

            matchExpression.push("red");
            sizeExpression.push(12);

            const stationsMid = data.filter(function(d){
                return +d["days_since_daily"] < 30 && d["station"] != least.station;
            }).map(d => d.station);

            const stationsBase = data.filter(function(d){
                return +d["days_since_daily"] > 29 && d["station"] != least.station;
            }).map(d => d.station);

            map.setPaintProperty('acis-reproj-7ytdey-mid-layer', 'text-color', "#000000");
            map.setPaintProperty('acis-reproj-7ytdey', 'text-color', "#000000");
            map.setPaintProperty('state-boundaries copy', 'fill-color', "#e8e8e8");


            map.setLayoutProperty('acis-reproj-7ytdey-top-layer', 'text-field', matchExpression);
            map.setLayoutProperty('acis-reproj-7ytdey-mid-layer', 'text-field', matchExpression);
            map.setLayoutProperty('acis-reproj-7ytdey', 'text-field', matchExpression);

            map.setLayoutProperty('acis-reproj-7ytdey-mid-layer', 'text-size', sizeExpression);
            map.setLayoutProperty('acis-reproj-7ytdey', 'text-size', sizeExpression);

            map.setFilter('acis-reproj-7ytdey-top-layer', [ "all", [ "match", ["get", "station"], [least.station], true, false ] ]);
            map.setFilter('acis-reproj-7ytdey-mid-layer', [ "all", [ "match", ["get", "station"], stationsMid, true, false ] ]);
            map.setFilter('acis-reproj-7ytdey', [ "all", [ "match", ["get", "station"], stationsBase, true, false ] ]);


            let leastDate = new Date(least.days_since_daily_date.slice(0,4), +least.days_since_daily_date.slice(5,7) -1, least.days_since_daily_date.slice(8,10));
            console.log("hiiii")
            if(!popup) {

                popup = new mapbox.Popup({ focusAfterOpen: false, maxWidth: '340px' });

                popup
                    .setLngLat([least.longitude,least.latitude])
                    //.setHTML("hi")
                    .setHTML(`${least.days_since_daily} days ago, it was ${least.days_since_val}°F in <b>${least.name}, ${least.state_abbr}</b> on <b>${formatTime(leastDate)}</b>. That is the hottest ${formatTimeNoYear(leastDate)} on record for the city.`)
                    .addTo(map);

                popupAdded = true;
            }

            

            map.on('mousemove', (e) => {
                const features = map.queryRenderedFeatures(e.point);
                
                // Limit the number of properties we're displaying for
                // legibility and performance
                const displayProperties = [
                    'type',
                    'properties',
                    'id',
                    'layer',
                    'source',
                    'sourceLayer',
                    'state'
                ];

                const displayFeatures = features.map((feat) => {
                    const displayFeat = {};
                    displayProperties.forEach((prop) => {
                        displayFeat[prop] = feat[prop];
                    });
                    return displayFeat;
                }).filter(function(d){
                    return d.layer.id.includes("acis")
                });

                if(displayFeatures.length > 0 && popupAdded){

                    popupData = displayFeatures[0].properties;

                    // if (hoveredStation !== null) {
                    //     map.setFeatureState(
                    //         { source: 'acis-reproj-7ytdey', id: hoveredStation },
                    //         { hover: false }
                    //     );
                    // }

                    // hoveredStation = popupData.station;
                    // map.setFeatureState(
                    //     { source: 'acis-reproj-7ytdey', id: hoveredStation },
                    //     { hover: true }
                    // );

                    if(hoveredStation !== popupData.station){
                        popup.remove();

                        map.removeFeatureState({
                            source: 'test-circles',
                            id: hoveredStation
                        });

                        hoveredStation = stationIds.indexOf(popupData.station);



                        map.setFeatureState({
                            source: 'test-circles',
                            id:hoveredStation
                        },
                        {
                            hover: true
                        });


                        // map.setFilter('circles', [ "all", [ "match", ["get", "station"], [popupData.station], true, false ] ]);
                        console.log("circling")

                        popup
                            .setLngLat([popupData.longitude,popupData.latitude])
                            .setHTML(`${popupData.days_since_daily} days ago, it was ${popupData.days_since_val}°F in <b>${popupData.name}, ${popupData.state_abbr}</b> on <b>${formatTime(popupDataDate)}</b>, the hottest ${formatTimeNoYear(popupDataDate)} on record for the city.`)
                            .addTo(map)
                    }
                }

                else {
                    map.setFeatureState(
                    {
                        source: 'test-circles',
                        id: hoveredStation
                    },
                    {
                        hover: false
                    }
                    );
                    hoveredStation = null;
                    popup.remove();
                }


            });

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
<div class="mobile-pop-up">
    <p class="mobile-pop-up-content">{popupData.days_since_daily} days ago, it was {popupData.days_since_val}°F in <b>{popupData.name}, {popupData.state_abbr}</b> on <b>{formatTime(popupDataDate)}</b>, the hottest {formatTimeNoYear(popupDataDate)} on record for the city.</p>
</div>

<style>
	.mapbox-container {
		width: 100%;
        height: 60vw;
        overflow: visible;
        max-height: 650px;
	}

    .mobile-pop-up {
        display: none;
        width: calc(100% - 20px);
        margin: 0 auto;
        box-shadow: 0 0 6px 0 rgba(0,0,0,.35);
        margin-bottom: 1rem;
        margin-top: 1rem;
        padding: .5rem;
        min-height: 100px;
    }

    .mobile-pop-up-content {
        font-family:'Yantramanav';
        font-size: 18px;
        text-align: center;
        line-height: 1.2;
        margin: 0;
        align-self: center;
    }

    @media only screen and (max-width: 500px) {
        .mapbox-container {
            height: 70vw;
        }
        .mobile-pop-up {
            display: flex;
        }
    }
</style>
