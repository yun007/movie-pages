<script>
	import {Router, Link, Route} from "svelte-routing";
	
	import Home from './routes/Gate.svelte';
	import Search from './routes/Search.svelte';
	import Sub1 from './routes/Sub1.svelte';
	import Footer from './components/Footer.svelte';
	

	export let Url ='';
	const imgUrl = 'https://image.tmdb.org/t/p/original'; 

</script>

<Router url="{Url}">
	<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
		<div class="container-fluid">
		  <a class="navbar-brand" href="/">Movie</a>
		  <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarColor01" aria-controls="navbarColor01" aria-expanded="false" aria-label="Toggle navigation">
			<span class="navbar-toggler-icon"></span>
		  </button>
		  <div class="collapse navbar-collapse" id="navbarColor01">
			<ul class="navbar-nav me-auto mb-2 mb-lg-0">
				<li class="nav-item"><Link to="/" class="nav-link active" aria-current="page">Home</Link></li>
				<li class="nav-item"><Link to="Search" class="nav-link">Search Movie</Link></li>
				<!-- <li class="nav-item"><Link to="Sub1" class="nav-link">Sub1</Link></li> -->
			</ul>
			<!-- <form class="d-flex">
			  <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
			  <button class="btn btn-outline-light" type="submit">Search</button>
			</form> -->
		  </div>
		</div>
	  </nav>


	<div class='content'>		
		<Route path="Search" imgUrl={imgUrl} component="{Search}"></Route>
		<!-- <Route path="Sub1" component="{Sub1}"></Route> -->
		<Route path="/"><Home imgUrl={imgUrl} /></Route>		
	</div>	
</Router>

<Footer />

