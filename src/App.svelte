<script>

    import Child from './Child.svelte';
    import RecipePage from './RecipePage.svelte'
    import SearchResults from "./SearchResults.svelte";


    export let name;
    $: suggestions = [];
    let visible = {
        child: false,
        else: true
    }
    let search = '';
    let promise = fetchSuggestions();
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

    async function fetchSuggestions() {
        if (search.length === 1) {
            suggestions = []
            const data = await fetch(`https://www.themealdb.com/api/json/v1/1/search.php?f=${search}`);
            let result = await data.json();
            $: result.meals.forEach(element => suggestions = [...suggestions, element.strMeal])
            console.log(suggestions)
        }
    }
</script>

<main>
    <div class="flex items-center justify-between relative  mx-auto h-28 bg-red-500">
        <span class="text-4xl text-white font-semibold align-middle ml-4">
            Recipe Page
            <img class="h-14 inline-block" src="/chef.png" alt="">
        </span>
        <span class="h-12">
            <input class="h-full shadow-inner transition rounded focus:rounded-sm  duration-100 border border-transparent focus:outline-none focus:ring-2 focus:ring-red-600 focus:border-transparent"
                   bind:value={search} on:input={fetchSuggestions} list="suggestions">
                <datalist id="suggestions">
                    {#each suggestions as el}
                        <option value="{el}">
                    {/each}
                </datalist>
            <button class="rounded-full shadow-md h-full bg-red-600 text-white w-24 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:ring-opacity-50 "
                    type="button" on:click={fetchMeal(search)}>Search</button>
        </span>
    </div>
</main>

{#if !searchResults}
    <h5> No search results</h5>
{:else if searchResults !== true }
    <SearchResults {searchResults}/>
{/if}
{#if meal}
    <RecipePage {meal}/>
{/if}

<style global>
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>