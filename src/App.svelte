<script>

/// imports 
		import { afterUpdate, onMount } from "svelte";
		import Navbar from "./Navbar.svelte"
		import Movie from "./Movie.svelte";
		import AddMovie from "./AddMovie.svelte"


// main state 
		 let movies = [];
		 let doUpdate = false;
	

		/// fetch all movies onMount
		onMount(() => {
			// TOGGLA DOUPDATE FÖR ATT KÖRA KEY
			doUpdate = !doUpdate
			fetch("http://localhost:3000/movies")
			.then(res => res.json())
			.then(data => { 
				movies = data;
				console.log("onMount", movies)
			})


		})
		
/// Functioner

		/// fetch Single movie 
		const fetchMovie = async (id) => {
		const res = await fetch(`http://localhost:3000/movies/${id}`)
		const data = await res.json();
		//console.log(data)
		return data;
	}


		/// remover movie
		const removeMovie = async (e) => {
		await fetch(`http://localhost:3000/movies/${e.detail}`, {
			method: "DELETE",
		});

		movies = movies.filter(movie => movie.id !== e.detail)
	}

		/// toggleBorrow
		const toggleBorrow = async (e) => {
	//console.log(e.detail.checked, e.detail.id)
		// hämta
		const movieTotoggle = await fetchMovie(e.detail.id);
	//console.log(movieTotoggle)
		//ändra
		const updateMovie = {...movieTotoggle, checked: !movieTotoggle.checked}
		console.log(updateMovie)

		/// fetch 
		const res = await fetch(`http://localhost:3000/movies/${e.detail.id}`, {
		method: "PUT",
		headers: {
			"Content-type": "application/json",
		},
		body: JSON.stringify(updateMovie)
		});
		const data = await res.json();
	//console.log("data", data)

		movies.map((movie) => 
		movie.id === data.id ?  {...movie, checked: data.checked } : movie)

		}

		/// addMovie
		const addMovie = (e) => {
	//console.log(e.detail)
			const newMovie = e.detail;


		fetch("http://localhost:3000/movies",{
		method: "post",
		headers: {
			"Content-type": "application/json"
		},
		body: JSON.stringify(newMovie)
		})
		.then(res => res.json())
		.then(data => console.log("success", data))
		.catch(err => console.log("err", err))

		movies = [...movies, newMovie]
		}

</script>


<Navbar />

<div class="container">
	<AddMovie on:addmovie={addMovie}/>

{#if movies.length === 0}
	<p> No movies </p>
	{:else}

	{#key doUpdate}
	{#each movies as movie}
	<Movie 
	movie={movie}
	on:removemovie={removeMovie} 
	on:checked={toggleBorrow}/>
	{/each}
	{/key}
{/if}

	
</div>

