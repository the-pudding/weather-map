<script>
  import Demo from "$components/helpers/Demo.svelte";
  import WIP from "$components/helpers/WIP.svelte";
  import Map from '$components/Map.svelte';
  import data from '$data/acis-agg-3.csv'
  import { select, minIndex } from "d3";
  import Footer from "$components/Footer.svelte";
  import { onMount } from "svelte";

  onMount(async () => {
    if(window.navigator.userAgent.match(/Android/i)){
      select(".led-fg").classed("android-adjust",true);
      select(".led-bg").classed("android-adjust",true);
    }
  })
  


  let leastIndex = minIndex(data, d => +d['days_since_daily']);

  let least = data[leastIndex];

</script>

<svelte:head>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Yantramanav:wght@400;700;900&display=swap" rel="stylesheet">
</svelte:head>

<div class="wrapper">
  <div class="byline byline-top">
    <p>NOTICE from management at <a href="/">The Pudding</a></p>
  </div>
  <div class="top-bar-wrapper">
    <div class="notice-bar">
      <div class="byline">
        <p>NOTICE from management at <a href="/">The Pudding</a></p>
      </div>

      <div class="updates byline">
        <p>This updates daily, as of March 10, 2022.</p>
      </div>

      <div class="safety-wrapper">
        <div class="safety">
          <svg width="23" height="23" viewBox="0 0 23 23" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="8.02271" y="0.356201" width="7.13197" height="22.1437" fill="white"/>
            <rect x="0.516846" y="14.9941" width="7.13201" height="22.1437" transform="rotate(-90 0.516846 14.9941)" fill="white"/>
          </svg>
          <p>SAFETY FIRST!</p>
          <svg width="23" height="23" viewBox="0 0 23 23" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="8.02271" y="0.356201" width="7.13197" height="22.1437" fill="white"/>
            <rect x="0.516846" y="14.9941" width="7.13201" height="22.1437" transform="rotate(-90 0.516846 14.9941)" fill="white"/>
          </svg>
        </div>
        <p class="highlight">Safety First!</p>

        <!-- <p class="highlight">Record-HIGH TEMPERATURES are preventable!</p> -->
      </div>
      
      <img src="assets/images/sweat.png" alt="icon of person sweating" />

      
    </div>
    <div class="top-bar">
      <div class="top-title">
        <p class="left">The U.S. has gone</p>
        <div class="led">
          <p class="led-fg">{+least.days_since_daily < 100 ? "0".concat(least.days_since_daily) : least.days_since_daily} days</p>
          <p class="led-bg">
            888 8888
          </p>
        </div>
        <!-- <p class="right">since A city SET a record daily temperature high</p> -->
        <p class="right">without a city breaking a record daily temperature high</p>
      </div>
    </div>
  </div>
  
  <div class="map-wrapper">
    <div class="map-title">
      <p class="">Days without a Daily Record High Temperature</p>
    </div>
    <Map lat={35} lon={-84} zoom={3.5} data={data} least={least}>
    </Map>
  </div>
  <div class="method">
    <p class="method-big">
      Record-high temperatures are preventable.
    </p>
    <div class="method-small">
      <p>As the average temperature of the earth increases, the climate becomes less stable. <a href="https://probablefutures.org/heat/a-tour-of-temperature/" target="_blank">Probable Futures</a> says it well: in a warmer world, the &ldquo;average occurs less frequently, and the extremes are wider.&ldquo;</p>
      <p>The organization has a helpful <a href="https://probablefutures.org/heat/a-tour-of-temperature/" target="_blank">example</a> that stresses why seasonal temperature highs are just as important as all-time records, &ldquo;As the earth’s climate has warmed, extremes reached in places that already experience a wide range of temperatures are becoming even more volatile in predictable ways. Since the big change isn’t the amount of energy coming in from the sun, summers are only slightly warmer, while spring, fall, and especially winter are much warmer. It’s less that the Arctic is getting hotter and more that it is losing its cold. For example, the historical average high temperature for December 1 was -15°C (5°F). On December 1, 2020 Fairbanks [Alaska] registered a high temperature of 3.9°C (39°F).&rdquo;</p>
      <p>A New York Times <a href="https://www.nytimes.com/interactive/2022/01/11/climate/record-temperatures-map-2021.html" target="_blank">analysis</a> of heat records found that, &ldquo;Temperatures in the United States [in 2020] set more all-time heat and cold records than any other year since 1994.&rdquo;
    </div>

    <div class="desc">
      <p>This updates daily, as of March 10</p>
      <p>Temperature records are collected from <a href="http://www.rcc-acis.org/docs_webservices.html" target="_blank">ACIS</a>, which tracks weather for approximately 400 U.S. cities. ACIS has data from the <a href="http://threadex.rcc-acis.org/" target="_blank">ThreadEx</a> project: &ldquo;ThreadEx is a project designed to address the fragmentation of station information over time due to station relocations for the express purpose of calculating daily extremes of temperature and precipitation. There are often changes in the siting of instrumentation for any given National Weather Service/Weather Bureau location over the observational history in a given city/region. As a result, obtaining a long time series (i.e., one hundred years or more) for computation of extremes is difficult, unless records from the various locations are 'threaded' or put together...In consultation with NOAA's National Centers for Environmental Information (NCEI) and the National Weather Service (NWS), the Northeast Regional Climate Center (NRCC) has evaluated station relocations and built "threads" for 270 locations that are published in NCEI's Local Climatological Data using NOAA daily data sets.&rdquo;</p>
      <p>You can browse this data on <a href="http://threadex.rcc-acis.org/" target="_blank">Threaded Extremes.</a></p>
    </div>
  </div>
</div>



<style>

  .method {
    border: 3px solid black;
    border-top: none;
    padding: 1rem;
    background-color: black;
  }

  .method p, .method a {
    color: white;
  }

  .method-big {
    font-size: 36px;
    font-weight: 700;
    margin-bottom: 1rem;
  }

  .method .desc {
    display: block;
  }

  .method .desc p {
    line-height: 1.4;
  }

  .method-small {
    margin-bottom: 1rem;
  }

  .method-small p {
    font-size: 18px;
    font-weight: 500;
    margin-bottom: 1rem;
    line-height: 1.3;
  }

  .desc p {
    margin-bottom: 1rem;
    font-size: 16px;
  }

  p {
    margin: 0;
    font-family: 'Yantramanav';
  }
  .top-bar-wrapper {
    display: flex;
    flex-wrap: wrap;
    position: relative;
    width: 100%;
  }

  .notice-bar {
    display: flex;
    background: white;
    border: 3px solid black;
    border-right: none;
    padding-left: 1rem;
    min-width: 380px;
    position: relative;

    width: 100%;
    border: none;
    justify-content: center;
    height: 80px;
    padding: 0;

  }

  .notice-bar p {
    text-transform: uppercase;
    font-weight: 900;
    font-size: 50px;
    align-self: center;
  }

  .safety {
    background: black;
    display: flex;
    border-radius: 10px;
    flex-grow: 1;
    margin-top: 4px;
    justify-content: center;
    max-width: 270px;
    display: none;
  }

  .safety svg {
    align-self: center;
  }

  .safety p {
    color: white;
    font-size: 27px;
    padding-right: .5rem;
    padding-left: .5rem;
  }

  .notice-bar .byline {
    /* display: none; */
    align-self: center;
    position: absolute;
    left: 0;
    max-width: 200px;
    display: block;

    
  }

  .notice-bar .updates {
    right: 0;
    left: auto;
    text-align: right;
  }

  .notice-bar img{
    width: 112px;
    position: absolute;
    right: 0;
    display: none;
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
    max-width: 1300px;
    margin-bottom: 1rem;

  }

  .top-bar {
    background: #0156A6;
    background: #000;
    display: flex;
    justify-content: space-between;
    border: 3px solid black;
    padding: 5px;
    padding-right: 1rem;
    flex-grow: 1;
    justify-content: flex-start;
  }

  .top-title {
    display: flex;
    justify-content: center;
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

  p.highlight {
    font-size: 15px;
    line-height: 1.1;
    width: 300px;
    text-align: center;
    margin: 0 auto;
    margin-top:5px;

    background: black;
    display: inline-block;
    color: white;
    border-radius: 10px;
    font-size: 22px;
    font-weight: 600;
    padding: .3rem 1.5rem;
    align-self: center;
    letter-spacing: 2px;
    width: auto;
    margin: 0;
  }

  .top-bar p {
    color: white;
    font-size: 24px;
    font-weight: 900;
  }

  .top-bar .left {
    text-align: right;
    font-size: 28px;
    padding-left: 1rem;
    /* max-width: 200px; */

    font-size: 30px;
    line-height: 1;
    padding: 0;
  }

  .top-bar .right {
    text-align: left;
    font-size: 20px;
    /* max-width: 270px; */
    line-height: 1.1;

    font-size: 22px;
    line-height: 1.3;
  }

  .top-bar .right, .top-bar .left {
       width: calc(50% - 180px);
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
    text-shadow: 0 0 4px #e4793e;
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
    position: relative;
    
    margin: 0 auto;
    z-index: 1000;
    width: 100%;
    pointer-events: none;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
  }

  .map-title p {
    text-align: center;
    font-size: 26px;
    font-family:'Yantramanav';
    font-weight: 600;
    text-transform: uppercase;
    line-height: 1;
    margin: 0 auto;
    
    text-shadow: -3px -3px 1px rgba(255,255,255,.40), -3px -2px 1px rgba(255,255,255,.40), -3px -1px 1px rgba(255,255,255,.40), -3px 0px 1px rgba(255,255,255,.40), -3px 1px 1px rgba(255,255,255,.40), -3px 2px 1px rgba(255,255,255,.40), -3px 3px 1px rgba(255,255,255,.40), -2px -3px 1px rgba(255,255,255,.40), -2px -2px 1px rgba(255,255,255,.40), -2px -1px 1px rgba(255,255,255,.40), -2px 0px 1px rgba(255,255,255,.40), -2px 1px 1px rgba(255,255,255,.40), -2px 2px 1px rgba(255,255,255,.40), -2px 3px 1px rgba(255,255,255,.40), -1px -3px 1px rgba(255,255,255,.40), -1px -2px 1px rgba(255,255,255,.40), -1px -1px 1px rgba(255,255,255,.40), -1px 0px 1px rgba(255,255,255,.40), -1px 1px 1px rgba(255,255,255,.40), -1px 2px 1px rgba(255,255,255,.40), -1px 3px 1px rgba(255,255,255,.40), 0px -3px 1px rgba(255,255,255,.40), 0px -2px 1px rgba(255,255,255,.40), 0px -1px 1px rgba(255,255,255,.40), 0px 1px 1px rgba(255,255,255,.40), 0px 2px 1px rgba(255,255,255,.40), 0px 3px 1px rgba(255,255,255,.40), 1px -3px 1px rgba(255,255,255,.40), 1px -2px 1px rgba(255,255,255,.40), 1px -1px 1px rgba(255,255,255,.40), 1px 0px 1px rgba(255,255,255,.40), 1px 1px 1px rgba(255,255,255,.40), 1px 2px 1px rgba(255,255,255,.40), 1px 3px 1px rgba(255,255,255,.40), 2px -3px 1px rgba(255,255,255,.40), 2px -2px 1px rgba(255,255,255,.40), 2px -1px 1px rgba(255,255,255,.40), 2px 0px 1px rgba(255,255,255,.40), 2px 1px 1px rgba(255,255,255,.40), 2px 2px 1px rgba(255,255,255,.40), 2px 3px 1px rgba(255,255,255,.40), 3px -3px 1px rgba(255,255,255,.40), 3px -2px 1px rgba(255,255,255,.40), 3px -1px 1px rgba(255,255,255,.40), 3px 0px 1px rgba(255,255,255,.40), 3px 1px 1px rgba(255,255,255,.40), 3px 2px 1px rgba(255,255,255,.40), 3px 3px 1px rgba(255,255,255,.40);
    font-size: 22px;
    font-weight: 900;
    
    padding-top: 1.5rem;
    /* transform: translate(0,20px); */
  }
  .byline-top {
      display: none;
  }

  .top-bar-wrapper {
    flex-wrap: wrap;
  }
  .notice-bar {
    width: 100%;
    border: none;
    justify-content: center;
    height: 80px;
    padding: 0;
  }

  .notice-bar img {
    display: none;
  }

  .top-bar .right {
    font-size: 22px;
    line-height: 1.3;
    max-width: none;
  }

  .top-bar .left {
    font-size: 30px;
    line-height: 1;
    padding: 0;
    max-width: none;
  }

  p.highlight {
    background: black;
    display: inline-block;
    color: white;
    border-radius: 10px;
    font-size: 22px;
    font-weight: 600;
    padding: .3rem 1.5rem;
    align-self: center;
    letter-spacing: 2px;
    width: auto;
    margin: 0;
  }

  .safety-wrapper {
    align-self: center;
  }

  .notice-bar .byline p {
    font-size: 15px;
    line-height: 1.2;
    font-weight:600;
    text-align: left;
  }

  .notice-bar .updates p {
    text-align: right;
  }

  .top-bar .right, .top-bar .left {
      width: calc(50% - 180px);
  }

  .safety {
    display: none;
  }

  .byline-top {
    display: none;
  }

  @media only screen and (max-width: 1150px) {
    .top-bar .right {
      font-size: 20px;
    }

    .top-bar .left {
      font-size: 26px;
    }

    p.highlight {
      font-size: 20px;
    }

    .notice-bar {
      height: 60px;
    }
    .notice-bar .byline {
      display: none;
    }

    .byline {
      display: block;
      margin: 0 auto;
    }
  }

  @media only screen and (max-width: 900px) {
    .top-bar .right {
      font-size: 18px;
    }

    .method-big {
      font-size: 28px;
    }

    .method .desc p {
      font-size: 16px;
    }

    .top-bar .left {
      font-size: 22px;
    }
    .top-bar .led {
      min-width: 220px;
    }
    .top-bar .led p {
      font-size: 55px;
    }
    .top-bar .right, .top-bar .left {
       width: auto;
       max-width: none;
    }
  }

  @media only screen and (max-width: 800px) {
    .top-title {
      flex-wrap: wrap;
    }
    .top-bar .right {
      font-size: 18px;
      width: 100%;
      text-align: center;
      padding: 1rem;
    }

    .top-bar .left {
      font-size: 22px;
      width: 100%;
      text-align: center;
      padding: .5rem;
    }

    .method-big {
      font-size: 24px;
      margin-bottom: 1rem;
    }

    .method .desc p {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 600px) {
    p.highlight {
      font-size: 16px;
    }
    
  }

  @media only screen and (max-width: 500px) {

    .method {
      /* background: white; */
      border-width: 1px;
    }

    .method p, .method a {
      color: black;
      color: white;
    }

    .method-big {
      font-size: 24px;
      margin-bottom: 1rem;
      line-height: 1.1;
    }

    .method .desc p {
      font-size: 12px;
      margin-bottom: .5rem;
    }

    .byline {
      margin-top: .5rem;
    }

    .map-title {
      position: relative;
      width:calc(100% - 10px);
      margin: 0 auto;
      pointer-events: none;
    }

    .map-wrapper {
      border-width: 1px;
    }

    .map-title p {
      letter-spacing: 0;
      text-transform: none;
      padding-top: .8rem;
      font-size: 18px;
      font-weight: 600;
      max-width: 200px;
      margin-bottom: -20px;
      pointer-events: none;
    }

    .wrapper {
      width: 100%;
      margin: 0;
      border-radius: 0;
      padding: .3rem;
    }

    /* p.highlight {
      line-height: 1.1;
      font-weight: 900;
      width: 100%;
      background: none;
      color: black;
      letter-spacing: 0;
      font-size: 25px;
    } */

    p.highlight {
      line-height: 1.1;
      font-weight: 900;
      width: 100%;
      font-size: 22px;
    }
    .notice-bar {
      margin-bottom: 1rem;
      margin-top: .4rem;
      min-width: auto;
    }
  }

</style>

<!-- <WIP /> -->
<!-- <Demo /> -->


<Footer />
