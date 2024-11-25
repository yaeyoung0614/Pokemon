<template>
  <div>
    <select v-model="pokemon1Name">
      <option disabled value="">Select First Pokémon</option>
      <option
        v-for="pokemon in sortedPokemonList"
        :key="pokemon.name"
        :value="pokemon.name"
      >
        {{ pokemon.name }}
      </option>
    </select>

    <select v-model="pokemon2Name">
      <option disabled value="">Select Second Pokémon</option>
      <option
        v-for="pokemon in sortedPokemonList"
        :key="pokemon.name"
        :value="pokemon.name"
      >
        {{ pokemon.name }}
      </option>
    </select>

    <button :disabled="!canBattle" @click="fetchSelectedPokemon">
      Battle!
    </button>
    <div v-if="loading" class="loading-indicator">Loading...</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "PokemonSelector",
  props: {
    pokemonList: Array,
  },
  data() {
    return {
      pokemon1Name: "",
      pokemon2Name: "",
      loading: false,
    };
  },
  computed: {
    sortedPokemonList() {
      return [...this.pokemonList].sort((a, b) => a.name.localeCompare(b.name));
    },
    canBattle() {
      return this.pokemon1Name && this.pokemon2Name;
    },
  },
  methods: {
    async fetchSelectedPokemon() {
      try {
        this.loading = true;
        const pokemon1 = await axios.get(
          `https://pokeapi.co/api/v2/pokemon/${this.pokemon1Name.toLowerCase()}`
        );
        const pokemon2 = await axios.get(
          `https://pokeapi.co/api/v2/pokemon/${this.pokemon2Name.toLowerCase()}`
        );
        this.$emit("selectPokemon", pokemon1.data, pokemon2.data);
      } catch (error) {
        alert("Error fetching Pokémon! Check names and try again.");
      } finally {
        this.loading = false;
      }
    },
    handleEnterKey(event) {
      if (event.key === "Enter" && this.canBattle) {
        this.fetchSelectedPokemon();
      }
    },
  },
  mounted() {
    window.addEventListener("keyup", this.handleEnterKey);
  },
  beforeUnmount() {
    window.removeEventListener("keyup", this.handleEnterKey);
  },
};
</script>

<style>
select {
  margin: 10px;
  padding: 5px;
}
button {
  padding: 5px 10px;
  cursor: pointer;
}
button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}
.loading-indicator {
  margin-top: 10px;
  font-weight: bold;
  color: #f39c12;
}
</style>
