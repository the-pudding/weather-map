<script>
  import Demo from "$components/helpers/Demo.svelte";
  import WIP from "$components/helpers/WIP.svelte";
  import Map from '$components/Map.svelte';
  import data from '$data/acis-agg-3.csv'
  import { minIndex } from "d3";

  let leastIndex = minIndex(data, d => +d['days_since_daily']);

  let least = data[leastIndex];

</script>

<svelte:head>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Yantramanav:wght@400;700;900&display=swap" rel="stylesheet">
</svelte:head>

<div class="wrapper">
  <div class="byline">
    <p>From the staff at <a href="">The Pudding</a></p>
  </div>
  <div class="top-bar-wrapper">
    <div class="notice-bar">
      <p>Notice!</p>
      <img src="assets/images/sweat.png" alt="icon of person sweating" />
    </div>
    <div class="top-bar">
      <div class="top-title">
        <p class="left">This country has gone</p>
        <div class="led">
          <p class="led-fg">{+least.days_since_daily < 100 ? "0".concat(least.days_since_daily) : least.days_since_daily} days</p>
          <p class="led-bg">
            888 8888
          </p>
        </div>
        <p class="right">since A city SET a record daily temperature high</p>
      </div>
    </div>
  </div>
  
  <div class="map-wrapper">
    <div class="map-title">
      <p class="">days without a daily record high temperature</p>
    </div>
    
    <Map lat={35} lon={-84} zoom={3.5} data={data} least={least}>
    </Map>
  </div>
</div>


<style>
  p {
    margin: 0;
    font-family: 'Yantramanav';
  }
  .top-bar-wrapper {
    display: flex;
    width: 100%;
  }
  .notice-bar {
    display: flex;
    background: white;
    border: 3px solid black;
    border-right: none;
    padding-left: 2rem;
  }

  .notice-bar p {
    text-transform: uppercase;
    font-weight: 900;
    font-size: 50px;
    align-self: center;
  }

  .notice-bar img{
    width: 112px;
  }
  .byline {
    text-align: center;
    text-transform: uppercase;
    font-weight: 600;
    font-size: 15px;
    margin-bottom: .5rem;
  }

  .byline a {
    color: black;
  }

  .wrapper {
    padding:1rem;
    box-shadow: 0 0 15px 5px rgba(0,0,0,.15);
    width: calc(100% - 2rem);
    margin: 0 auto;
    margin-top: 1rem;
    border-radius: 10px;
    background:white;
    padding-top: .5rem;
  }

  .top-bar {
    background: #0156A6;
    display: flex;
    justify-content: space-between;
    border: 3px solid black;
    padding: 5px;
    padding-right: 1rem;
    flex-grow: 1;
  }

  .top-title {
    display: flex;
  }

  .led {
    display: block;
    min-width: 300px;
    border: 4px solid white;
    box-shadow: 0 0 4px 0 rgba(0,0,0,.15);
    border-radius: 2px;
    background: #372417;
    position: relative;
    margin-right: 17px;
    margin-left: 17px;
  }

  .top-bar p {
    color: white;
    font-size: 28px;
    font-weight: 900;
  }

  .top-bar .left {
    max-width: 200px;
    text-align: right;
  }

  .top-bar .right {
    max-width: 340px;
    text-align: left;
  }

  .top-bar .led p {
    font-size: 76px;
    font-family: 'Digital 7 Mono';
    margin: 0;
    color: rgba(255,168,37,.14);
    line-height: .7;
    padding: .3rem;
    padding-top: .7rem;
    padding-bottom: .1rem;
  }

  .top-bar .led .led-fg {
    color: #ffad57;
    text-shadow: 0 0 14px #e4793e;
    position: relative;
    z-index: 100;
  }

  .top-bar .led .led-bg {
    position: absolute;
    text-shadow: 0 0 14px rgba(212,116,64,.18);
    top: 0;
    left: 0;
    z-index: 0;
  }

  .top-title p {
    text-transform: uppercase;
    align-self: center;
    line-height: 1;
  }

  .map-wrapper {
    border: 3px solid black;
    border-top: none;
    position: relative;
  }

  .map-title {
    position: absolute;
    top: 0rem;
    left: 0;
    right: 0;
    margin: 0 auto;
    z-index: 1000;
    width: 100%;
    pointer-events: all;
  }

  .map-title p {
    text-align: center;
    font-size: 26px;
    font-family:'Yantramanav';
    font-weight: 600;
    max-width: 340px;
    text-transform: uppercase;
    line-height: 1;
    margin: 0 auto;
    padding-top: 3rem;
    text-shadow: -3px -3px 1px rgba(255,255,255,.40), -3px -2px 1px rgba(255,255,255,.40), -3px -1px 1px rgba(255,255,255,.40), -3px 0px 1px rgba(255,255,255,.40), -3px 1px 1px rgba(255,255,255,.40), -3px 2px 1px rgba(255,255,255,.40), -3px 3px 1px rgba(255,255,255,.40), -2px -3px 1px rgba(255,255,255,.40), -2px -2px 1px rgba(255,255,255,.40), -2px -1px 1px rgba(255,255,255,.40), -2px 0px 1px rgba(255,255,255,.40), -2px 1px 1px rgba(255,255,255,.40), -2px 2px 1px rgba(255,255,255,.40), -2px 3px 1px rgba(255,255,255,.40), -1px -3px 1px rgba(255,255,255,.40), -1px -2px 1px rgba(255,255,255,.40), -1px -1px 1px rgba(255,255,255,.40), -1px 0px 1px rgba(255,255,255,.40), -1px 1px 1px rgba(255,255,255,.40), -1px 2px 1px rgba(255,255,255,.40), -1px 3px 1px rgba(255,255,255,.40), 0px -3px 1px rgba(255,255,255,.40), 0px -2px 1px rgba(255,255,255,.40), 0px -1px 1px rgba(255,255,255,.40), 0px 1px 1px rgba(255,255,255,.40), 0px 2px 1px rgba(255,255,255,.40), 0px 3px 1px rgba(255,255,255,.40), 1px -3px 1px rgba(255,255,255,.40), 1px -2px 1px rgba(255,255,255,.40), 1px -1px 1px rgba(255,255,255,.40), 1px 0px 1px rgba(255,255,255,.40), 1px 1px 1px rgba(255,255,255,.40), 1px 2px 1px rgba(255,255,255,.40), 1px 3px 1px rgba(255,255,255,.40), 2px -3px 1px rgba(255,255,255,.40), 2px -2px 1px rgba(255,255,255,.40), 2px -1px 1px rgba(255,255,255,.40), 2px 0px 1px rgba(255,255,255,.40), 2px 1px 1px rgba(255,255,255,.40), 2px 2px 1px rgba(255,255,255,.40), 2px 3px 1px rgba(255,255,255,.40), 3px -3px 1px rgba(255,255,255,.40), 3px -2px 1px rgba(255,255,255,.40), 3px -1px 1px rgba(255,255,255,.40), 3px 0px 1px rgba(255,255,255,.40), 3px 1px 1px rgba(255,255,255,.40), 3px 2px 1px rgba(255,255,255,.40), 3px 3px 1px rgba(255,255,255,.40)
  }

</style>

<!-- <WIP /> -->
<!-- <Demo /> -->


<!-- <Footer /> -->
