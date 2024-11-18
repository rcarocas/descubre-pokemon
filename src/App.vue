<template>
  <div id="app"  class="text-bg-dark">
    <header class="text-center">
      <img src="./assets/Pokemon.svg" alt="" />
      <h2>¿Quién es ese Pokémon?</h2>
      <p>Pokemones descubiertos: <span>{{ counter }}</span></p>
    </header>
    <main class="overflow-auto container">
      <div class="row">
        <div class="col-12 col-md-4 col-lg-3" v-for="(pokemon, index) in pokemons" :key="index">
          <PokemonCard :pokemon="pokemon" @rightAnswer="counterInc"/>
        </div>
      </div>
    </main></div>
</template>

<script>
import axios from "axios";
import PokemonCard from "./components/PokemonCard.vue";

export default {
  name: "App",
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
      counter: 0
    };
  },
  methods: {
    async getPokemon() {
      try {
        let response = await axios.get("https://pokeapi.co/api/v2/pokemon?offset=130&limit=20");
        this.pokemons = response.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    counterInc(){
      this.counter++ 
    }
  },
  mounted() {
    this.getPokemon();
  },
};
</script>

<style>
*{
  margin: 0;
  padding: 0;
  font-family: "Quicksand", sans-serif;
}

main{
  height: 60vh;
}


</style>
