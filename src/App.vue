<template>
  <div id="app">
    <header>
      <img src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pokémon_logo.svg" alt="Pokémon Logo" class="logo" />
      <h2>¿Quién es ese Pokémon?</h2>
    </header>
    <div class="pokemon-container">
      <PokemonCard 
        v-for="pokemon in pokemons" 
        :key="pokemon.id" 
        :pokemon="pokemon" 
        @reveal="increaseCounter"
      />
    </div>
    <p class="counter">Pokémons descubiertos: {{ revealedCount }}</p>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  components: {
    PokemonCard
  },
  data() {
    return {
      pokemons: [],
      revealedCount: 0
    };
  },
  created() {
    this.fetchAllPokemons();
  },
  methods: {
    fetchAllPokemons() {
      axios.get('https://pokeapi.co/api/v2/pokemon?limit=1000')
        .then(response => {
          const results = response.data.results;
          const randomPokemons = this.getRandomElements(results, 20);
          const promises = randomPokemons.map(pokemon => axios.get(pokemon.url));
          Promise.all(promises)
            .then(responses => {
              this.pokemons = responses.map(response => response.data);
            });
        })
        .catch(error => {
          console.error('Error fetching Pokémon data:', error);
        });
    },
    getRandomElements(arr, count) {
      const shuffled = arr.slice();
      let i = arr.length;
      const min = i - count;
      let temp, index;

      while (i > min) {
        index = Math.floor((i--) * Math.random());
        temp = shuffled[i];
        shuffled[i] = shuffled[index];
        shuffled[index] = temp;
      }

      return shuffled.slice(min);
    },
    increaseCounter() {
      this.revealedCount++;
    }
  }
};
</script>

<style>
#app {
  text-align: center;
  font-family: 'Arial', sans-serif;
  background-color: #f0f8ff;
  padding: 20px;
}

header {
  margin: 20px 0;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.logo {
  width: 300px;
  height: auto;
}

header h2 {
  font-size: 2.5em;
  font-weight: bold;
  color: #2a75bb;
  margin-top: 10px;
}

.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.pokemon-card {
  margin: 10px;
}

.counter {
  font-size: 1.5em;
  margin-top: 20px;
  color: #333;
}
</style>








