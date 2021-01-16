<script>


	import * as d3 from 'd3';
	import { onDestroy } from 'svelte';
	import Team from '../components/Team.svelte'

	import nbateams from '../utils/nbateams.js'
	import colors from '../utils/color.js'
	import {totals} from '../utils/stores.js'

	let totals_value = [];

	const unsubscribe = totals.subscribe(v => {
		totals_value = v;
	});



	let years = ['2020/21', '2021/22','2022/23','2023/24','2024/25']
	
	let value = years[0];

	// $: sortedTeams = function(){
	// 	console.log(totals_value)


	// 	if(totals_value.length === 30){
	// 		nbateams.forEach((d,i) => {d.value = totals_value[i][value]})
	// 		console.log(nbateams.sort(function(a, b) {return b.value - a.value }))
	// 		nbateams.sort(function(a, b) {return b.value - a.value })
	// 	}
	// 		return nbateams
	// }


	onDestroy(unsubscribe);
</script>

<main>
	<h1>NBA Salaries</h1>
	<select bind:value>
		{#each years as item}
			<option value={item}>{item}</option> 
		{/each}
	</select>
	<br/>
	{#each nbateams as t}
		<Team year={value} years={years} colors={colors[t[0]]} team={t[1]}/>
	{/each}
</main>


<style>
	main {
        background: whitesmoke;
		padding: 0;
	}
</style>