<template lang="html">
<div>
<!-- <pokemon-poke v-for="(pokemon,index) in allPokemon" :key='index' :pokemon='pokemon' /> -->
</div>
</template>

<script>
import PokemonPoke from './PokemonPoke.vue'
export default {
  name: 'pokemon-list',
  components: {
    'pokemon-poke': PokemonPoke
  },
  props: ['allPokemon']
}
</script>

<style lang="css" scoped>
</style>
