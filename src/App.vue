<template lang="html">
<div>
<h1> Kanto Pokedex </h1>
<pokemon-list :allPokemon="pokemonData" />
<pokemon-search :allPokemon="pokemonData" />
<pokemon-detail :pokemon="selectedPokemon" v-if="selectedPokemon"/>
</div>
</template>

<script>
import PokemonList from './components/PokemonList.vue';
import PokemonSearch from './components/PokemonSearch.vue';
import PokemonDetail from './components/PokemonDetail.vue'
import {eventBus} from './main.js';

export default {
  name: 'app',
  data() {
    return {
    allPokemon: [],
    pokemonData: [],
    selectedPokemon: null
  }
},
mounted() {
  fetch('https://pokeapi.co/api/v2/pokemon/?limit=151')
  .then(res => res.json())
  .then(data => {
    this.allPokemon = data.results;
    this.getPokemonData();
  })

  eventBus.$on('pokemon-selected', (pokemon) => {
    this.selectedPokemon = pokemon;
  })
},

methods: {
  getPokemonData: function() {
    for (const pokemon of this.allPokemon) {
      let url = pokemon.url;

      fetch(url)
      .then(res => res.json())
      .then(data => this.pokemonData.push(data))
    }
  }
},
components: {
  'pokemon-list': PokemonList,
  'pokemon-search': PokemonSearch,
  'pokemon-detail': PokemonDetail
}
}
</script>

<style lang="css" scoped>
h1 {
  display: flex;
  justify-content: center;
}
</style>
