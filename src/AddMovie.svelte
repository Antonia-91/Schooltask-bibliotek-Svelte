<script>
/// imports 
import { createEventDispatcher } from "svelte";

/// variabler
let movie = {
        name: "",
        type:"",
        description:"",
        img: "",
        points: 0,
        checked: true
    }

    const dispatch = createEventDispatcher();

/// functioner 
const onSubmit = (e) => {
    e.preventDefault();
    console.log("movie", movie )
    dispatch("addmovie", movie)
    movie = {
        name: "",
        type:"",
        description:"",
        img: "",
        points: 0,
        checked: true
    }
}

</script>

<form class="grid-3" on:submit={onSubmit}>
    <input type="text" placeholder="title..." bind:value={movie.name}>
    <input type="text" placeholder="action..." bind:value={movie.type}>
    <input type="text" placeholder="description" bind:value={movie.description}>
    <input type="text" placeholder="image-URL" bind:value={movie.img}>
    <input type="number" placeholder="movie points" bind:value={movie.points}>
    <input class="checkox" type="checkbox" checked={movie.checked} value={movie.checked} >
    <input type="submit" value="Add Movie" class="btn btn-primary">
    </form>