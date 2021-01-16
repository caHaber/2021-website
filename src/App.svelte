<script>
	import { Router, Route } from "svelte-routing";
	import Nba from './routes/Nba.svelte';
	import Home from './routes/Home.svelte';


  	import NavLink from "./components/NavLink.svelte";
  // Used for SSR. A falsy value is ignored by the Router.
  	export let url = "";
</script>
<!-- svelte-ignore empty-block -->
<main>
	<Router url="{url}">
		<div>
		  <NavLink name="Home" to="/"></NavLink>
		  <NavLink name="NBA" to="nba"></NavLink>
		</div>
		<div>
		  <Route path="/" component="{Home}"/>
	
		  <Route path="nba">
			  <Nba/>
		  </Route>
		</div>
	</Router>
</main>

  
<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;	
	}


	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>