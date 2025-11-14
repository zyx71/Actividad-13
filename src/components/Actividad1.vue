<script setup>
import { ref } from 'vue'

// Ejercicio 1: Búsqueda de Pokémon
const pokemonName = ref('')
const pokemonData = ref(null)
const loading = ref(false)
const error = ref('')

const buscarPokemon = async () => {
  if (!pokemonName.value.trim()) {
    error.value = 'Por favor ingresa el nombre de un Pokémon'
    return
  }

  loading.value = true
  error.value = ''
  pokemonData.value = null

  try {
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName.value.toLowerCase()}`)
    
    if (!response.ok) {
      throw new Error('Pokémon no encontrado')
    }

    const data = await response.json()
    pokemonData.value = {
      name: data.name,
      image: data.sprites.front_default
    }
  } catch (err) {
    error.value = err.message || 'Error al buscar el Pokémon'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div>
    <h2>Ejercicio 1: Buscar Pokémon</h2>
    
    <div class="form-container">
      <div class="form-group">
        <label for="pokemon-name">Nombre del Pokémon:</label>
        <input 
          id="pokemon-name"
          type="text" 
          v-model="pokemonName"
          placeholder="Ej: pikachu, charizard, bulbasaur..."
          @keyup.enter="buscarPokemon"
        />
      </div>

      <button @click="buscarPokemon" :disabled="loading">
        {{ loading ? 'Buscando...' : 'Buscar Pokémon' }}
      </button>

      <p v-if="error" class="error">{{ error }}</p>

      <div v-if="pokemonData" class="pokemon-result">
        <h3>{{ pokemonData.name.toUpperCase() }}</h3>
        <img :src="pokemonData.image" :alt="pokemonData.name" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.form-container {
  max-width: 500px;
  margin: 20px 0;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

button {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

button:hover:not(:disabled) {
  background-color: #45a049;
}

.error {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}

.pokemon-result {
  margin-top: 30px;
  text-align: center;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  border: 2px solid #4CAF50;
}

.pokemon-result h3 {
  color: #4CAF50;
  margin-bottom: 15px;
  font-size: 24px;
}

.pokemon-result img {
  width: 200px;
  height: 200px;
  image-rendering: pixelated;
}
</style>
