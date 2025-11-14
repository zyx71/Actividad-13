<script setup>
import { ref } from 'vue'
import Actividad1 from './components/Actividad1.vue'
import Actividad2 from './components/Actividad2.vue'

const vistaActual = ref('actividad1')

const cambiarVista = (vista) => {
  vistaActual.value = vista
}
</script>

<template>
  <div>
    <h1>Actividades Vue.js</h1>
    
    <nav>
      <button 
        @click="cambiarVista('actividad1')"
        :class="{ active: vistaActual === 'actividad1' }"
      >
        Actividad 1
      </button>
      <button 
        @click="cambiarVista('actividad2')"
        :class="{ active: vistaActual === 'actividad2' }"
      >
        Actividad 2
      </button>
    </nav>

    <div class="contenido">
      <Actividad1 v-if="vistaActual === 'actividad1'" />
      <Actividad2 v-if="vistaActual === 'actividad2'" />
    </div>
  </div>
</template>

<style scoped>
h1 {
  text-align: center;
  margin: 20px 0;
}

nav {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 20px 0;
  padding: 20px;
  background-color: #f5f5f5;
  border-radius: 8px;
}

nav button {
  padding: 12px 24px;
  font-size: 16px;
  border: 2px solid #ddd;
  background-color: white;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
}

nav button:hover {
  background-color: #e0e0e0;
}

nav button.active {
  background-color: #4CAF50;
  color: white;
  border-color: #4CAF50;
}

.contenido {
  padding: 20px;
}
</style>
