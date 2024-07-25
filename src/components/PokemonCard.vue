<template>
    <div class="pokemon-card">
      <div class="image-container">
        <img :src="pokemonImage" :class="{ revealed: revealed }" />
      </div>
      <div v-if="!revealed" class="guess-section">
        <input type="text" v-model="userGuess" @keyup.enter="checkGuess" placeholder="Nombre del Pokémon" />
        <button @click="checkGuess">Descubrir</button>
      </div>
      <div v-else class="revealed-section">
        <p>{{ pokemonName }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['pokemon'],
    data() {
      return {
        userGuess: '',
        revealed: false
      };
    },
    computed: {
      pokemonName() {
        return this.pokemon.name;
      },
      pokemonImage() {
        return this.pokemon.sprites.other['official-artwork'].front_default || this.pokemon.sprites.front_default;
      }
    },
    methods: {
      checkGuess() {
        if (this.userGuess.toLowerCase() === this.pokemonName.toLowerCase()) {
          this.revealed = true;
          this.$emit('reveal');
        } else {
          alert('Nombre incorrecto. Intenta nuevamente.');
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .pokemon-card {
    display: inline-block;
    margin: 20px;
    text-align: center;
    border: 1px solid #ccc;
    border-radius: 10px;
    padding: 10px;
    background-color: #fff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .pokemon-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
  
  .image-container {
    width: 150px;
    height: 150px;
    margin: 0 auto;
  }
  
  .pokemon-card img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    filter: blur(5px) grayscale(100%);
    transition: filter 0.3s;
  }
  
  .pokemon-card img.revealed {
    filter: none;
  }
  
  .guess-section {
    margin-top: 10px;
  }
  
  .guess-section input {
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  
  .guess-section button {
    padding: 5px 10px;
    margin-left: 10px;
    background-color: #2a75bb;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .guess-section button:hover {
    background-color: #1a4f88;
  }
  
  .revealed-section {
    font-weight: bold;
    font-size: 1.2em;
    color: green;
    margin-top: 10px;
  }
  </style>
  
  
  
  
  
  