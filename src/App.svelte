<script>

    import RecipePage from './RecipePage.svelte'
    import SearchResults from "./SearchResults.svelte";
    import Header from "./Header.svelte";


    $: meal = undefined;
    $: searchResults = true;

    async function fetchMeal(input) {
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


    function handleChildAction(event) {
        fetchMeal(event.detail)
    }
</script>

<Header on:search={handleChildAction}/>

{#if !searchResults}
    <h5> No search results</h5>
{:else if searchResults !== true }
    <SearchResults on:meal={handleChildAction} {searchResults}/>
{/if}
{#if meal}
    <RecipePage {meal}/>
{/if}

<style global>
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>