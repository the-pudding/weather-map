<script>
    export let dataset;

    let value;

    $: value = dataset;

</script>

<select aria-label="record-type" bind:value>
    <option value="daily">daily</option>
    <option value="all-time">all-time</option>
</select>

<style>
select {
    pointer-events: all;
    padding-top: 0;
    font-size: 1.2em;
    padding-bottom: 0;
    padding-left: 0.25em;
    font-family: inherit;
    font-weight: 600;
    text-transform: uppercase;
    background-color: black;
    color: white;
    appearance: auto;
    -webkit-writing-mode: horizontal-tb !important;
    text-rendering: auto;
    color: -internal-light-dark(black, white);
    letter-spacing: normal;
    word-spacing: normal;
    line-height: normal;
    text-transform: none;
    text-indent: 0px;
    text-shadow: none;
    display: inline-block;
    text-align: start;
    appearance: auto;
    box-sizing: border-box;
    align-items: center;
    white-space: pre;
    -webkit-rtl-ordering: logical;
    background-color: -internal-light-dark(rgb(255, 255, 255), rgb(59, 59, 59));
    cursor: default;
    margin: 0em;
    border-width: 1px;
    border-style: solid;
    border-color: -internal-light-dark(rgb(118, 118, 118), rgb(133, 133, 133));
    border-image: initial;
    border-radius: 0px;
}

select:not(:-internal-list-box) {
    overflow: visible !important;
}
</style>
