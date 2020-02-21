<template lang="html">
<div class="card">
  <p class="name">{{pokemon.name}}</p>
  <p> Base Experience: {{pokemon.base_experience}} </p>
  <div class='picture'>
  <img :src="pokemon.sprites.front_default" />
 </div>
  <p>Type</p>
  <ul>
    <li v-for="(type, index) in pokemon.types" :key="index" class='name'> {{type.type.name}} </li>
  </ul>
  <p>Moves</p>
  <ul>
    <li v-for="(ability, index) in pokemon.abilities" :key="index" class='name'> {{ability.ability.name}} </li>
  </ul>
</div>

</template>

<script>
export default {
  name: 'pokemon-detail',
  props: ['pokemon']
}
</script>

<style lang="css" scoped>
.card {
  height: 430px;
  max-width: 300px;
  background: #39394a;
  border-radius: 6px;
  padding: 20px;
  border: 5px solid #9494a4;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  margin-top: 10px;
}

.name {
  text-transform: capitalize;
}

.picture {
  background: white;
}

p {
  text-align: center;
  font-weight: bold;
}

ul {
  padding-left: 125px;
}

img {
 padding-left: 100px;
}

</style>
