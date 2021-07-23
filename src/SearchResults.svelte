<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    export let searchResults=[];
    function sendClicked(input) {
        dispatch ('meal',input )
    }
</script>

<div class="mx-auto grid grid-flow-row gap-4 p-4 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 rounded bg-yellow-50 lg:max-w-screen-2xl ">
    {#each searchResults as results }

        <div class="h-auto cursor-pointer p-2 flex flex-col p-3 inline-block transition duration-100 hover:bg-yellow-100 " on:click={sendClicked(results.strMeal)}>
            <img class=" inline-block block w-1/2 mx-auto rounded-full shadow" src="{results.strMealThumb}"
                 alt="A picture of the {results.strMeal}">
            <div class="flex-auto ">
                <h1 class="text-center text-3xl ">
                    {results.strMeal}
                </h1>
                <p class="text-center" >
                    {results.strArea}
                </p>
            </div>

        </div>
    {/each}
</div>