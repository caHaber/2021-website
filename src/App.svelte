<script>

	import Page from './routes/Page.svelte';
	import Home from './routes/Home.svelte';

	import { Router, Route } from "svelte-routing";
  	import NavLink from "./components/NavLink.svelte";
  // Used for SSR. A falsy value is ignored by the Router.
  	export let url = "";
</script>
<!-- svelte-ignore empty-block -->

<Router url="{url}">
	<nav>
	  <NavLink to="/">Home</NavLink>
	  <NavLink to="page">Page</NavLink>
	</nav>
	<div>
	  <Route path="/" component="{Home}"/>

	  <Route path="page">
		  <Page name="Blog post page 1"/>
	  </Route>
	</div>
</Router>
  
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