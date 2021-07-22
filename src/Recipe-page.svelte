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


<section transition:fade>

    <h1>
        {meal.strMeal}
    </h1>
    <h4>
        {meal.strArea} {meal.strCategory}
    </h4>
    <img src="{meal.strMealThumb}" alt="A picture of the {meal.strMeal}">

    <table>
        {#each ingredients as {name, measure}}
            <tr>
                <td> {name}</td>
                <td> {measure}</td>
            </tr>
        {/each}
    </table>

    <p class="instructions">
        {meal.strInstructions}
    </p>

</section>

<style>
    .instructions {
        white-space: pre-wrap;
    }
</style>