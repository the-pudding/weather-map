<script>
  import Demo from "$components/helpers/Demo.svelte";
  import WIP from "$components/helpers/WIP.svelte";
  import Map from '$components/Map.svelte';
  import data from '$data/acis-agg-3.csv'
  import { minIndex } from "d3";
  import Footer from "$components/Footer.svelte";

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
        <p class="highlight">Record-HIGH TEMPERATURES are avoidable!</p>
      </div>
      
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
        <!-- <p class="right">since A city SET a record daily temperature high</p> -->
        <p class="right">without a city setting a record daily temperature high</p>
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
      Report all records to your supervisor.
    </p>
    <div class="desc">
      <p>This project updates daily.</p>
      <p>Weather records are collected from <a href="http://www.rcc-acis.org/docs_webservices.html">ACIS</a>, which uses &ldquo;threaded&rdquo; records for approximately 400 U.S. cities. Proximate weather stations are combined to give a historic record for a particular city (weather stations are often decommissioned and moved to different locations).</p>
      <p>You can browse this data on <a href="http://threadex.rcc-acis.org/">Threaded Extremes.</a></p>
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
  }

  .method .desc {
    display: block;
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
    display: none;
  }

  .notice-bar img{
    width: 112px;
    position: absolute;
    right: 0;
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
    max-width: 200px;
  }

  .top-bar .right {
    text-align: left;
    font-size: 20px;
    max-width: 270px;
    line-height: 1.1;
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
    pointer-events: all;

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

  @media only screen and (max-width: 1300px) {
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

    .notice-bar .byline {
      align-self: center;
      position: absolute;
      left: 0;
      max-width: 200px;
      display: block;
      
    }

    .notice-bar .byline p {
      font-size: 15px;
      line-height: 1.2;
      font-weight:600;
      text-align: left;
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
      background: white;
      border-width: 1px;
    }

    .method p, .method a {
      color: black;
    }

    .method-big {
      font-size: 18px;
      margin-bottom: 1rem;
    }

    .method .desc p {
      font-size: 12px;
      margin-bottom: .5rem;
      line-height: 1.1;
    }

    .byline {
      margin-top: .5rem;
    }

    .map-title {
      position: relative;
      width:calc(100% - 10px);
      margin: 0 auto;
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
    }

    .wrapper {
      width: 100%;
      margin: 0;
      border-radius: 0;
      padding: .3rem;
    }

    p.highlight {
      line-height: 1.1;
      font-weight: 900;
      width: 100%;
      background: none;
      color: black;
      letter-spacing: 0;
      font-size: 25px;
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
