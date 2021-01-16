<script>
    import { onMount, onDestroy } from "svelte";
    import * as d3 from "d3";
    import TeamNode from "./TeamNode.svelte";
    import { totals } from "../utils/stores.js";

    export let team;
    export let colors;
    export let year;
    export let years;

    let data;
    let total_raw_data;
    let totals_value = [];

    const MAX_HEIGHT = 400;
    const MIN_HEIGHT = 200;
    const RATIO = 1.5

    const unsubscribe = totals.subscribe((v) => {
        totals_value = v;
    });

    $: createDimensions = function(totals_arr) {
        let scale = d3
            .scaleLinear()
            .domain(d3.extent(totals_arr, (t) => t[year]))
            .range([MIN_HEIGHT, MAX_HEIGHT]);
            
        let w_h = scale(total_raw_data[year]);
        return { width: w_h, height: w_h * RATIO };
    }

    function parseData(d) {
        years.forEach((y) => {
            let val = d[y];
            d[y] = +val.substring(1, val.length);
        });
        return d;
    }

    onMount(async () => {
        const res = await d3.csv(`/data/teams/${team}.csv`, parseData);
        totals.update((arr) => [...arr, res[res.length - 1]]);
        total_raw_data = res[res.length - 1];
        data = res.splice(0, res.length - 1);
    });

    onDestroy(() => { totals.update((arr) => []); return unsubscribe});
</script>

<div class="team">
    {#if data && totals_value.length === 30}
        <TeamNode
            dimensions={createDimensions(totals_value)}
            {data}
            {colors}
            {team}
            bind:year />
    {/if}
</div>
<style>
div.team {
	display: inline-block;
	padding:10px;
	width:auto;
	vertical-align: top; 
}

</style>
