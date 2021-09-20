<script>
    /// imports 
	import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    /// props
    export let movie = {};
	 let showInfo = false; 

      /// functions
      const onDelete = () => dispatch("removemovie", movie.id)
      const change = () => dispatch("checked", {checked: movie.checked, id: movie.id})
      const toggle = () => (showInfo = !showInfo);
</script>

<style>


</style>


<div class="card" id="{movie.id}">
    <div class="flex">
        <h1>{movie.name}</h1>
        <h3>{movie.type}</h3>
        <div class="borrow_&_delete">
            <button class="btn btn-sm" on:click="{change}">
                {#if movie.checked} Borrow {:else} Return {/if}
              </button>
            <button class="btn btn-danger btn-sm" on:click="{onDelete}">
                    X
            </button>
        </div>
    </div>
    <div class="img_div"><img src="{movie.img}" alt="" /></div>

    <p>{#if movie.checked} Avalible {:else} <span style="color: brown;">Not Avalible</span> {/if}</p>


    <button class="btn btn-sm" on:click="{toggle}">
        {#if showInfo} hide {:else} description... {/if}
    </button>

    {#if showInfo}
    <div class="more_info">
	<p>{movie.description}</p>
	</div>
	
	{/if}
</div>