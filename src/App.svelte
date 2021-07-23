<script>

    import RecipePage from './RecipePage.svelte'
    import SearchResults from "./SearchResults.svelte";
    import Header from "./Header.svelte";
    // import { onMount } from 'svelte';
    let home = true;
    $: meal = undefined;
    $: searchResults = true;
    // let categories = getLists("c");
    async function fetchMeal(input) {
        home=false;
        meal = undefined;
        searchResults = true;
        const data = await fetch(`https://www.themealdb.com/api/json/v1/1/search.php?s=${input}`);
        let result = await data.json()

        if (result.meals === null) {
            searchResults = '';
        } else if (result.meals != null && result.meals.length === 1) {
            meal = result.meals[0];
        } else {
            searchResults = result.meals;
        }
    }

    //  async function getLists(type){
    //     const data = await fetch(`https://www.themealdb.com/api/json/v1/1/list.php?${type}=list`);
    //     let result = await data.json()
    //     let tempArr=result.meals;
    //     return tempArr;
    //
    // }
    function handleHome(event){
        meal = undefined;
        searchResults = true;
        home = event.detail
    }
    function handleChildAction(event) {
        fetchMeal(event.detail)
    }
</script>

<Header on:home={handleHome} on:search={handleChildAction}/>

{#if !searchResults}
    <div class="container h-4/5 flex content-center mx-auto">
        <div class="shadow-md sm:w-3/4 md:w-1/3 rounded-full mx-auto bg-yellow-50 text-center">
            <h5 class="text-2xl"> No search results</h5>

        </div>
    </div>

{:else if searchResults !== true }
    <SearchResults on:meal={handleChildAction} {searchResults}/>
{/if}
{#if meal}
    <RecipePage {meal}/>
{:else if home}
    <div class="container h-4/5 flex content-center overflow-hidden mx-auto">
        <div class="shadow-md sm:w-3/4 p-10 md:w-1/3 mx-auto rounded-lg bg-yellow-50 m-auto text-center">
            <h5 class="text-2xl mb-5">Welcome at Recipes</h5>
            <p class="text-lg "> Find recipes by their name, and enjoy cooking!
            </p>

        </div>
    </div>

{/if}

<style global>
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>