<template>
  <div>
    <h2>Battle Result</h2>
    <img
      v-if="winner"
      :src="winner.sprites.front_default"
      :alt="winner.name"
      class="winner-icon"
    />
    <div class="battle-stats">
      <p>{{ pokemon1.name }} Total Stats: {{ totalStats1 }}</p>
      <p>{{ pokemon2.name }} Total Stats: {{ totalStats2 }}</p>
    </div>
    <p>{{ battleResult }}</p>
  </div>
</template>

<script>
export default {
  name: "BattleResult",
  props: {
    pokemon1: Object,
    pokemon2: Object,
  },
  computed: {
    totalStats1() {
      return this.pokemon1.stats.reduce((sum, stat) => sum + stat.base_stat, 0);
    },
    totalStats2() {
      return this.pokemon2.stats.reduce((sum, stat) => sum + stat.base_stat, 0);
    },
    battleResult() {
      const typeAdvantage = this.getTypeAdvantage(
        this.pokemon1.types,
        this.pokemon2.types
      );
      const randomFactor = Math.random() < 0.1 ? 1 : 0; // 10% chance of random advantage

      if (typeAdvantage === 1 || randomFactor === 1) {
        return `${this.pokemon1.name} wins due to type advantage or luck!`;
      } else if (typeAdvantage === -1 || randomFactor === -1) {
        return `${this.pokemon2.name} wins due to type advantage or luck!`;
      }

      if (this.totalStats1 > this.totalStats2) {
        return `${this.pokemon1.name} wins with ${this.totalStats1} total stats!`;
      } else if (this.totalStats2 > this.totalStats1) {
        return `${this.pokemon2.name} wins with ${this.totalStats2} total stats!`;
      } else {
        return "It's a tie!";
      }
    },
    winner() {
      if (this.totalStats1 > this.totalStats2) {
        return this.pokemon1;
      } else if (this.totalStats2 > this.totalStats1) {
        return this.pokemon2;
      } else {
        return null;
      }
    },
  },
  methods: {
    getTypeAdvantage() {
      // Placeholder logic for type advantage
      // You can add more complex logic based on type relationships
      return 0;
    },
  },
};
</script>

<style>
h2 {
  margin-top: 20px;
  font-family: "JetBrains Mono", monospace;
}
.battle-stats {
  margin-bottom: 10px;
}
.winner-icon {
  margin-top: 10px;
  width: 100px;
  height: 100px;
}
p {
  font-family: "JetBrains Mono", monospace;
}
</style>
