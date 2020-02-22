<template lang="html">
<div>
<div>
  <img src="../public/pokeball.png" height="40" width="40" class="bounce"/>
</div>
<h1 v-on:click="goHome"> Kanto Pokédex </h1>
<pokemon-list :allPokemon="pokemonData" />
<pokemon-search :allPokemon="pokemonData" />
<button name="Random Pokemon" class="random" type="button" v-on:click="randomPokemon">Click for a Random Pokémon!</button>
<button name="Previous Pokemon" type="button" class="previous" v-on:click="viewPrevious" v-if="selectedPokemon">Previous Pokémon</button>
<button name="Next Pokemon" type="button" class="next" v-on:click="viewNext" v-if="selectedPokemon">Next Pokémon</button>
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
  font-family: 'Acme';
  margin-top: -20px;
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

@keyframes bounce {
  from,
  20%,
  53%,
  80%,
  to {
    animation-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1);
    transform: translate3d(0, 0, 0);
  }

  40%,
  43% {
    animation-timing-function: cubic-bezier(0.755, 0.05, 0.855, 0.06);
    transform: translate3d(0, -20px, 0);
  }

  70% {
    animation-timing-function: cubic-bezier(0.755, 0.05, 0.855, 0.06);
    transform: translate3d(0, -10px, 0);
  }

  90% {
    transform: translate3d(0, -4px, 0);
  }
}

.bounce {
  animation-name: bounce;
  animation-duration: 4s;
  transform-origin: center bottom;
  animation-iteration-count: infinite;
}


</style>
