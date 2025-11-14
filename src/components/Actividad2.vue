<script setup>
import { ref, onMounted } from 'vue'

const kits = ref([])
const selectedKitId = ref('')
const newName = ref('')
const mensaje = ref('')
const loading = ref(false)

// URL de tu API Laravel - CAMBIAR POR TU ENDPOINT REAL
const API_URL = 'http://localhost:8000/api/robotics-kits'

// Cargar kits existentes al montar el componente
onMounted(async () => {
  await cargarKits()
})

// Obtener los IDs de kits existentes desde tu API Laravel
const cargarKits = async () => {
  try {
    loading.value = true
    
    // Si tu API Laravel está corriendo, descomenta esto:
    // const response = await fetch(API_URL)
    // const data = await response.json()
    // kits.value = data
    
    // Simulación temporal (eliminar cuando tengas tu API lista)
    kits.value = [
      { id: 1, name: 'Arduino Starter Kit' },
      { id: 2, name: 'Raspberry Pi Kit' },
      { id: 3, name: 'LEGO Mindstorms' },
      { id: 4, name: 'VEX Robotics Kit' },
      { id: 5, name: 'Makeblock mBot' }
    ]
    
    loading.value = false
  } catch (error) {
    console.error('Error al cargar kits:', error)
    mensaje.value = 'Error al cargar la lista de kits'
    loading.value = false
  }
}

// Enviar actualización usando PATCH
const actualizarKit = async () => {
  if (!selectedKitId.value || !newName.value.trim()) {
    mensaje.value = 'Por favor selecciona un kit e ingresa un nuevo nombre'
    return
  }

  try {
    loading.value = true
    mensaje.value = ''

    // Si tu API Laravel está corriendo, descomenta esto:
    /*
    const response = await fetch(`${API_URL}/${selectedKitId.value}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ name: newName.value })
    })

    if (!response.ok) {
      throw new Error('Error al actualizar el kit')
    }

    const data = await response.json()
    */

    // Simulación temporal del PATCH (eliminar cuando tengas tu API lista)
    console.log('PATCH Request:', {
      url: `${API_URL}/${selectedKitId.value}`,
      body: { name: newName.value }
    })

    await new Promise(resolve => setTimeout(resolve, 500))
    
    // Actualizar el kit local
    const kit = kits.value.find(k => k.id == selectedKitId.value)
    if (kit) {
      kit.name = newName.value
    }

    mensaje.value = `✓ Kit ${selectedKitId.value} actualizado exitosamente con el nombre: ${newName.value}`
    loading.value = false
    
    // Limpiar campos
    selectedKitId.value = ''
    newName.value = ''
  } catch (error) {
    console.error('Error al actualizar:', error)
    mensaje.value = 'Error al actualizar el kit de robótica'
    loading.value = false
  }
}
</script>

<template>
  <div>
    <h2>Ejercicio 2: Actualizar Kit de Robótica (PATCH)</h2>
    
    <div class="form-container">
      <div class="form-group">
        <label for="kit-id">Selecciona el ID del Kit:</label>
        <select 
          id="kit-id" 
          v-model="selectedKitId"
          :disabled="loading"
        >
          <option value="">-- Selecciona un Kit --</option>
          <option 
            v-for="kit in kits" 
            :key="kit.id" 
            :value="kit.id"
          >
            ID: {{ kit.id }} - {{ kit.name }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label for="new-name">Nuevo Nombre:</label>
        <input 
          id="new-name"
          type="text" 
          v-model="newName"
          placeholder="Escribe el nuevo nombre del kit..."
          :disabled="loading || !selectedKitId"
        />
      </div>

      <button 
        @click="actualizarKit" 
        :disabled="loading || !selectedKitId || !newName.trim()"
      >
        {{ loading ? 'Actualizando...' : 'Actualizar Kit (PATCH)' }}
      </button>

      <p v-if="mensaje" :class="{ 'success': mensaje.includes('✓'), 'error': mensaje.includes('Error') }">
        {{ mensaje }}
      </p>
    </div>

    <hr />

    <div class="kits-list">
      <h3>Kits de Robótica Actuales:</h3>
      <ul>
        <li v-for="kit in kits" :key="kit.id">
          <strong>ID {{ kit.id }}:</strong> {{ kit.name }}
        </li>
      </ul>
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

select, input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

select:disabled, input:disabled {
  background-color: #f5f5f5;
  cursor: not-allowed;
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

.success {
  color: green;
  font-weight: bold;
  margin-top: 10px;
}

.error {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}

hr {
  margin: 30px 0;
  border: 1px solid #ccc;
}

.kits-list {
  margin-top: 20px;
}

.kits-list ul {
  list-style: none;
  padding: 0;
}

.kits-list li {
  padding: 8px;
  border-bottom: 1px solid #eee;
}
</style>
