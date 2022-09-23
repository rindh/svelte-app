<script>
    import { onMount } from "svelte";
    let score = 0;
    let currentGuess = "";
    let pokemon = "";
    let avi = "";
    onMount(async () => {
        newPokemon();
    });

    function newPokemon() {
        let id = randomPokemonId();
        fetch("https://pokeapi.co/api/v2/pokemon/" + id)
            .then((response) => response.json())
            .then((data) => {
                avi = data.sprites["front_default"];
                pokemon = data;
            })
            .catch((error) => {
                console.log(error);
                return [];
            });
    }

    function randomPokemonId() {
        let min = Math.ceil(1);
        let max = Math.floor(905);
        return Math.floor(Math.random() * (max - min) + min);
    }

    function guess() {
        console.log(currentGuess + " : " + pokemon.name);
        if (currentGuess.toLocaleLowerCase().trim() == pokemon.name) {
            score++;
            newPokemon();
        }
    }
</script>

<div style="flex:10; display:flex; flex-flow:column">
    <img src={avi} alt="Italian Trulli" />
    <label style="font-size:40px; padding:10px">{score}</label>
    <form on:submit|preventDefault={guess}>
        <input type="text" bind:value={currentGuess} />
    </form>
</div>

<style>
    div {
        width: 655px;
        margin: auto;
    }
</style>
