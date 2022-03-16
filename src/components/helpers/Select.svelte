<script>
  export let options = [];
  export let label = "";
  export let selectClass = "";
  export let disabled = false;
  export let value = options.length ? options[0].value : "";

  const id = `select-${Math.floor(Math.random() * 1000000)}`;
</script>

<div class="select {selectClass}">
  {#if label}
    <label for={id}>{label}</label>
  {/if}
  <select {id} bind:value {disabled}>
    {#each options as option}
      <option>{option.label || option.value}</option>
    {/each}
  </select>
</div>

<style>


  .select {
    position: relative;
  }


  select {
    width: 100%;
    font-family: inherit;
    font-size: inherit;
    cursor: pointer;
  }

  .map-title label{
    font-size: 14px;
    position: absolute;
    left: 0;
    right: 0;
    margin: 0 auto;
    transform: translate(0,-100%);
  }

  .map-title {
    font-size: inherit;
    padding: 0rem;
    font-weight: 700;
  }

  .map-title select {
    background: white;
    color: black;
    border: 2px solid var(--color-gray-900);
    padding: .2em;
    padding-left: .5em;
    appearance: none;
    line-height: 1.4;
    text-transform: uppercase;
    font-weight: 700;
    padding-right: 2em;
  }

  label {
    display: inline-block;
    font-family: inherit;
    font-weight: inherit;
    font-size: 1em;
    margin-bottom: 0.25em;
  }


  select::-ms-expand {
    display: none;
  }

  .notice-title label {
    display: none;
  }

  

  .select::after {
    display: block;
    content: "";
    position: absolute;
    right: 0.75em;
    width: .5em;
    height: .5em;
    z-index: 1;
    background: var(--color-gray-900);
    clip-path: polygon(0% 0%, 100% 100%, 0% 100%);
    transform-origin: center center;
    transform: rotate(-45deg);
    pointer-events: none;
    bottom: 1em;
  }


  .notice-title select {
    background: #444;
    color: white;
    padding: .2em;
    padding-left: .5em;
    appearance: none;
    line-height: 1.4;
    text-transform: uppercase;
    font-weight: 700;
    padding-right: 2em;
  }
  .notice-title::after {
    background: #b1a7a7;
    bottom: .9em;
  }

  select:hover {
    background: var(--color-gray-100);
  }

  .notice-title select:hover {
    background: #444;
  }

  select:focus {
    box-shadow: 0 0 4px 0 var(--color-focus);
  }

  .select:disabled {
    cursor: not-allowed;
    background-color: var(--color-gray-300);
  }

  @media only screen and (max-width: 500px) {
    label {
      display: none;
    }

    .map-title select {
      padding-right: 2em;
      font-size: 16px;
      padding-top: 0;
      padding-bottom: 0;
      border-width: 1px;
    }

    .select::after {
      bottom: .7em;
    }
  }
</style>
