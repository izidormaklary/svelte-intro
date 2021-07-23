<script>
    let search = '';
    $: suggestions = [];
    async function fetchSuggestions() {
        if (search.length === 1) {
            suggestions = []
            const data = await fetch(`https://www.themealdb.com/api/json/v1/1/search.php?f=${search}`);
            let result = await data.json();
            $: result.meals.forEach(element => suggestions = [...suggestions, element.strMeal])
            console.log(suggestions)
        }
    }
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    function sendClicked(input) {
        dispatch ('search',input )
    }
    function Home(){
        dispatch('home',true)
    }
    const onKeyPress = e => {
        if (e.charCode === 13) sendClicked(search)
    }

</script>

<nav class="bg-red-500 sticky top-0 ">
    <div class="flex flex-wrap items-center sm:w-full  md:w-3/4 mx-auto justify-between h-28 ">

        <span class="cursor-pointer sm:text-1xl md:text-4xl flex-shrink-0 text-white font-semibold align-middle ml-4"
        on:click={Home}>
            Recipes
            <img class="h-14 inline-block" src="/chef.png" alt="">
        </span>
        <span class=" m-1 h-12 flex-shrink-0" on:click>
            <input class="h-full shadow-inner pl-1  rounded   duration-100 border border-transparent focus:w-auto focus:rounded-sm focus:outline-none focus:ring-2 focus:ring-red-600 focus:border-transparent"
                   bind:value={search}
                   on:input={fetchSuggestions}
                   on:keypress={onKeyPress}
                   placeholder="look up recipe.."
                   list="suggestions">
                <datalist id="suggestions">
                    {#each suggestions as el}
                        <option value="{el}">
                    {/each}
                </datalist>
            <button class="rounded-full shadow-md h-full bg-red-600 text-white w-24 hover:bg-red-700 outline-none focus:outline-none focus:ring-2 focus:ring-white-600 focus:ring-opacity-50 "
                    type="button" on:click={sendClicked(search)}>Search</button>
        </span>
    </div>
</nav>