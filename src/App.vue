<template>
  <div id="app">
    <h1>Pokémon Battle Simulator</h1>

    <!-- Dropdown to Filter Pokémon by Generation -->
    <select v-model="selectedGeneration" @change="fetchPokemonByGeneration">
      <option v-for="gen in generations" :key="gen.id" :value="gen.id">
        {{ gen.name }}
      </option>
    </select>

    <!-- Pokémon Selector -->
    <PokemonSelector
      :pokemonList="filteredPokemonList"
      @selectPokemon="handlePokemonSelection"
    />

    <!-- Display Pokémon and Battle Results -->
    <div v-if="pokemon1 && pokemon2">
      <div class="pokemon-display">
        <PokemonCard :pokemon="pokemon1" />
        <PokemonCard :pokemon="pokemon2" />
      </div>
      <BattleResult :pokemon1="pokemon1" :pokemon2="pokemon2" />
    </div>
  </div>
</template>

<script>
import PokemonSelector from "./components/PokemonSelector.vue";
import PokemonCard from "./components/PokemonCard.vue";
import BattleResult from "./components/BattleResult.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    PokemonSelector,
    PokemonCard,
    BattleResult,
  },
  data() {
    return {
      generations: [
        { id: 1, name: "Generation I" },
        { id: 2, name: "Generation II" },
        { id: 3, name: "Generation III" },
        { id: 4, name: "Generation IV" },
        { id: 5, name: "Generation V" },
        { id: 6, name: "Generation VI" },
        { id: 7, name: "Generation VII" },
        { id: 8, name: "Generation VIII" },
      ],
      selectedGeneration: 1,
      pokemonList: [],
      filteredPokemonList: [],
      pokemon1: null,
      pokemon2: null,
    };
  },
  methods: {
    async fetchPokemonByGeneration() {
      try {
        const response = await axios.get(
          `https://pokeapi.co/api/v2/generation/${this.selectedGeneration}/`
        );
        const pokemonSpecies = response.data.pokemon_species;

        // Sort Pokémon by ID
        const sortedById = pokemonSpecies.sort((a, b) => {
          const idA = parseInt(a.url.split("/").slice(-2, -1)[0]);
          const idB = parseInt(b.url.split("/").slice(-2, -1)[0]);
          return idA - idB;
        });

        // Sort Pokémon alphabetically by name for dropdown
        const sortedByName = sortedById.sort((a, b) => {
          return a.name.localeCompare(b.name);
        });

        this.pokemonList = sortedByName;
        this.filteredPokemonList = sortedByName;
      } catch (error) {
        console.error("Error fetching Pokémon by generation:", error);
      }
    },
    handlePokemonSelection(pokemon1, pokemon2) {
      this.pokemon1 = pokemon1;
      this.pokemon2 = pokemon2;
    },
  },
  mounted() {
    this.fetchPokemonByGeneration();
  },
};
</script>

<style>
body {
  background-color: #fff7f7;
  margin: 0;
  padding: 0;
}

h1 {
  font-family: "JetBrains Mono", monospace;
}

#app {
  text-align: center;
}

.pokemon-display {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  margin: 20px 0;
}
</style>
