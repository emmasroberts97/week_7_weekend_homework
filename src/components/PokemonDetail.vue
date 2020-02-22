<template lang="html">
<div class="card">
  <h2 class="name">{{pokemon.name}}</h2>
  <p> HP: {{getHP}}</p>
  <div class="picture">
  <img :src="pokemon.sprites.front_default" height="200" width="200" />
 </div>
 <div>
  <p class="underline" id="type">Type</p>
  <ul class="type">
    <li v-for="(type, index) in pokemon.types" :key="index" class='name'> {{type.type.name}} </li>
  </ul>
</div>
<div >
  <p class="underline" id="moves">Moves</p>
  <ul class="moves">
    <li v-for="(ability, index) in pokemon.abilities" :key="index" class='name'> {{ability.ability.name}} </li>
  </ul>
</div>
</div>

</template>

<script>
export default {
  name: 'pokemon-detail',
  props: ['pokemon', 'allPokemon'],
  computed: {
    getHP() {
    for (const stat of this.pokemon.stats) {
      if (stat.stat.name == "hp") {
        return stat.base_stat;
      }
    }
   }
 }
}
</script>

<style lang="css" scoped>
.card {
  height: 480px;
  max-width: 330px;
  background-image: url('../../public/poke_card.png');
  padding: 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  margin-top: 20px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.name {
  text-transform: capitalize;
  margin-top: -3px;
}

p {
  text-align: center;
  font-weight: bold;
}


img {
 margin-top: -25px;
 padding-left: 70px;
}

h2 {
  text-align: center;
}

.type {
  margin-bottom: -50px;
}

.moves {
  margin-left: 200px;
  margin-bottom: -50px;
}

.underline {
  text-decoration: underline;
}

#type {
  margin-left: -200px;
}

#moves {
  margin-top: -30px;
  margin-left: 225px;
}


</style>
