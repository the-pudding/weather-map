<script>
  import { csv } from "d3";
  import { onMount } from "svelte";
  import Wrapper from '$components/Wrapper.svelte';

  let data = null;
  let last;
  let lastDate;

  let url = "https://pudding.cool/2022/03/weather-map-data/acis.csv"

  onMount(async () => {
    csv(url).then(function(response) {
      data = response;
	  });
  })



</script>

  {#if data}
    <Wrapper data={data}></Wrapper>
  {/if}

<style>
</style>