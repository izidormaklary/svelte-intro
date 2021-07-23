<script>
    import {fade} from 'svelte/transition';

    export let meal;

    $: ingredients = meal ? findIngredients() : [];

    function findIngredients() {
        let tempArr = [];
        let i = 1;
        let nameOf = undefined;
        while (nameOf !== '') {
            nameOf = meal[`strIngredient${i}`];
            let measureOf = meal[`strMeasure${i}`]
            let ingredient = {name: nameOf, measure: measureOf}
            tempArr.push(ingredient)
            i++;
        }
        tempArr.pop()
        return tempArr;
    }


</script>


<div class="container h-full shadow-md bg-yellow-50 m-auto grid-flow-row">

    <div class="text-center py-4 sm:w-3/4 md:w-1/2 xlg:w-1/4 mx-auto shadow rounded-b-lg bg-gray-100 ">
        <h1 class="text-3xl" >
            <p></p>
            {meal.strMeal}
        </h1>
        <h4 class="tex-lg">
            {meal.strArea} {meal.strCategory}
        </h4>
    </div>

    <div class="grid h-100 sm:grid-cols-1 md:grid-cols-2 mt-4">

        <img class="w-3/4 mx-auto rounded-md shadow-lg" src="{meal.strMealThumb}" alt="A picture of the {meal.strMeal}"
             inline-block>

        <div class="w-3/4 mx-auto grid grid-cols-2 p-4  bg-gray-50 shadow-lg rounded-md">
            <div class="col-span-2 text-center">
                <h4 class="text-xl"> Ingredients:</h4>
            </div>

            {#each ingredients as {name, measure}}
                <div class="border-b-2 flex">
                    <p class="self-center mx-auto">{name}</p>
                </div>
                <div class="border-b-2 flex">
                    <p class="self-center mx-auto">{measure}</p>
                </div>

            {/each}
        </div>
        <div class="md:col-span-2 w-3/4 mx-auto my-5 shadow-sm rounded-lg p-5 bg-gray-50  " >
            <p class="whitespace-pre-wrap md:text-base lg:text-lg leading-loose text-gray-900">
                {meal.strInstructions}
            </p>
        </div>
    </div>


</div>
