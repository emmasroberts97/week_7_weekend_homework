<template lang="html">
<div>
<h1 v-on:click="goHome"> Kanto Pokedex </h1>
<pokemon-list :allPokemon="pokemonData" />
<pokemon-search :allPokemon="pokemonData" />
<button name="Random Pokemon" class="random" type="button" v-on:click="randomPokemon">Click for a Random Pokemon!</button>
<button name="Previous Pokemon" type="button" class="previous" v-on:click="viewPrevious" v-if="selectedPokemon">Previous Pokemon</button>
<button name="Next Pokemon" type="button" class="next" v-on:click="viewNext" v-if="selectedPokemon">Next Pokemon</button>
<pokemon-detail :pokemon="selectedPokemon" :allPokemon="pokemonData" v-if="selectedPokemon"/>
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
  },
  randomPokemon: function() {
    let pokemonIndex = Math.floor(Math.random() * (151 - 1)) + 1;
    for (const pokemon of this.pokemonData) {
      if (pokemon['id'] == pokemonIndex) {
        this.selectedPokemon = pokemon;
      }
    }
    this.reset();
  },
  goHome: function() {
    this.selectedPokemon = null;
    let dropDown = document.getElementById("dropdown");
    this.reset();
  },
  viewPrevious: function() {
    let newPokemon = this.pokemonData.find((pokemon) => pokemon.id === (this.selectedPokemon.id - 1))

    this.selectedPokemon = newPokemon;
    this.reset();
  },
  viewNext: function(){
  let newPokemon = this.pokemonData.find((pokemon) => pokemon.id === (this.selectedPokemon.id + 1))

  this.selectedPokemon = newPokemon;
  this.reset();
},
  reset: function() {
   let dropDown = document.getElementById("dropdown");
    dropDown.selectedIndex = 0;
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
  color: white;
}

button.random {
  display: block;
  margin-left: auto;
  margin-right: auto;
  margin-top: 10px;
}

button.next {
  margin-left: 650px;
}

button.previous {
margin-left: 200px;
}


</style>
