<template lang="html">

<!-- <div>
  <label for="pokemon-search">Search for Pokemon:</label>
  <input id="pokemon-search" name="pokemon-search" type="search" v-model="selectedPokemon"></input>

<div v-if="selectedPokemon != null">
<pokemon-detail v-for="(pokemon, index) in filteredPokemon" :key="index" :pokemon="pokemon" />
</div> -->

<select v-on:change="handleChange" v-model="selectedPokemon" id="dropdown">
  <option value="0" disabled selected>Choose a Pokémon:</option>
  <option v-for="(pokemon, index) in allPokemon" :key="index" :value="pokemon"> {{pokemon.name}} </option>
</select>
</div>

</template>

<script>
import PokemonDetail from './PokemonDetail.vue';
import {eventBus} from '../main.js';

export default {
  name: 'pokemon-search',
  data() {
    return {
      selectedPokemon: null
    }
  },
  props: ['allPokemon'],
  methods: {
    handleChange() {
      eventBus.$emit('pokemon-selected', this.selectedPokemon)
    }
  },
  computed: {
    filteredPokemon() {
      return this.allPokemon.filter(pokemon => {
      return pokemon.name.toLowerCase().includes(this.selectedPokemon.toLowerCase())
    })
  }
},
components: {
  'pokemon-detail': PokemonDetail
},
}
</script>

<style lang="css" scoped>
select {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>
