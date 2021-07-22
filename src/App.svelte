<script>

    import Child from './Child.svelte';
    import RecipePage from './Recipe-page.svelte'


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
        console.log(result)
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
<nav class="bg-blue-900 shadow-lg">
    <div class="container mx-auto">
        <div class="sm:flex">
            <a href="#" class="text-white text-3xl font-bold p-3">APP LOGO</a>

            <!-- Menus -->
            <div class="ml-55 mt-4">
                <ul class="text-white sm:self-center text-xl">
                    <li class="sm:inline-block">
                        <a href="#" class="p-3 hover:text-red-900">About</a>
                    </li>
                    <li class="sm:inline-block">
                        <a href="#" class="p-3 hover:text-red-900">Services</a>
                    </li>
                    <li class="sm:inline-block">
                        <a href="#" class="p-3 hover:text-red-900">Blog</a>
                    </li>
                    <li class="sm:inline-block">
                        <a href="#" class="p-3 hover:text-red-900">Contact</a>
                    </li>
                </ul>
            </div>

        </div>
    </div>
</nav>
<main>
    <form>
        <input bind:value={search} on:input={fetchSuggestions} list="suggestions">
        <datalist id="suggestions">
            {#each suggestions as el}
                <option value="{el}">
            {/each}
        </datalist>
        <button type="button" on:click={fetchMeal(search)}>Search</button>
    </form>
</main>
{#if !searchResults}
    <h5> No search results</h5>
{:else if searchResults !== true }
    {#each searchResults as results }

            <div class="h-20" on:click={fetchMeal(results.strMeal)} style="cursor: pointer;">
                <img class="h-full" src="{results.strMealThumb}" alt="A picture of the {results.strMeal}">
               <h5>{results.strMeal}</h5>

            </div>

    {/each}
{/if}
{#if meal}
    <RecipePage {meal}/>
{/if}

<!--<style>-->
<!--/*    main {*/-->
<!--/*        text-align: center;*/-->
<!--/*        padding: 1em;*/-->
<!--/*        max-width: 240px;*/-->
<!--/*        margin: 0 auto;*/-->
<!--/*    }*/-->

<!--/*    h1 {*/-->
<!--/*        color: #ff3e00;*/-->
<!--/*        text-transform: uppercase;*/-->
<!--/*        font-size: 4em;*/-->
<!--/*        font-weight: 100;*/-->
<!--/*    }*/-->
<!--/*    .results{*/-->
<!--/*        text-align: center;*/-->
<!--/*    }*/-->
<!--/*    .results>h5{*/-->
<!--/*        display: inline-block;*/-->
<!--/*    }*/-->
<!--/*    .thumbnail{*/-->
<!--/*align-self: start;*/-->
<!--/*        height: 100px;*/-->
<!--/*    }*/-->

<!--</style>-->
