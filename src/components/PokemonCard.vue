<template>
  <div :class="['pokemon-card', typeClass]">
    <h2>{{ pokemon.name }}</h2>
    <img :src="pokemon.sprites.front_default" :alt="pokemon.name" />
    <img :src="pokemon.sprites.back_default" :alt="pokemon.name" />
    <ul>
      <li v-for="stat in pokemon.stats" :key="stat.stat.name">
        {{ stat.stat.name }}: {{ stat.base_stat }}
      </li>
    </ul>
    <p>Type: {{ types }}</p>
    <p>Abilities: {{ abilities }}</p>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",
  props: {
    pokemon: Object,
  },
  computed: {
    typeClass() {
      return this.pokemon.types[0].type.name;
    },
    types() {
      return this.pokemon.types.map((type) => type.type.name).join(", ");
    },
    abilities() {
      return this.pokemon.abilities
        .map((ability) => ability.ability.name)
        .join(", ");
    },
  },
};
</script>

<style>
.pokemon-card {
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 10px;
  text-align: center;
  margin: 10px;
  width: 350px;
  height: 400px;
}
.pokemon-card.fire {
  background-color: #fddfdf;
}
.pokemon-card.water {
  background-color: #def3fd;
}
.pokemon-card.grass {
  background-color: #e0f8e0;
}
.pokemon-card.electric {
  background-color: #fcf7de;
}
.pokemon-card.ice {
  background-color: #def3fd;
}
.pokemon-card.fighting {
  background-color: #e6e0d4;
}
.pokemon-card.poison {
  background-color: #e0d4e6;
}
.pokemon-card.steel {
  background-color: #a0969b;
}
.pokemon-card.normal {
  background-color: #a0969b;
}
.pokemon-card.psychic {
  background-color: #dab7ca;
}
</style>
