<script setup>
import { ref, computed } from 'vue'

// Ejercicio 1: Foco
const isOn = ref(false)
const offImg = new URL('../fotos/foco_off.png', import.meta.url).href
const onImg = new URL('../fotos/foco_on.png', import.meta.url).href

const toggle = () => {
  isOn.value = !isOn.value
}

// Ejercicio 2: Estudiantes
const students = [
  { id: 1, firstName: "Lucía", lastName: "García", grade: 92 },
  { id: 2, firstName: "Mateo", lastName: "Hernández", grade: 78 },
  { id: 3, firstName: "Sofía", lastName: "Martínez", grade: 85 },
  { id: 4, firstName: "Diego", lastName: "López", grade: 67 },
  { id: 5, firstName: "Valentina", lastName: "Pérez", grade: 95 },
  { id: 6, firstName: "Daniel", lastName: "Ramírez", grade: 88 },
  { id: 7, firstName: "Camila", lastName: "Torres", grade: 73 },
  { id: 8, firstName: "Alejandro", lastName: "Flores", grade: 100 },
  { id: 9, firstName: "Isabella", lastName: "Ruiz", grade: 81 },
  { id: 10, firstName: "Sebastián", lastName: "Gómez", grade: 60 }
]

const searchQuery = ref('')

const filteredStudents = computed(() => {
  if (!searchQuery.value) {
    return students
  }
  return students.filter(student => 
    student.firstName.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <div>
    <!-- Ejercicio 1: Foco -->
    <section>
      <h2>Ejercicio 1: Control de Foco</h2>
      <div>
        <img :src="isOn ? onImg : offImg" alt="Foco" />
        <button @click="toggle">{{ isOn ? 'Apagar' : 'Encender' }}</button>
      </div>
    </section>

    <hr />

    <!-- Ejercicio 2: Tabla de Estudiantes -->
    <section>
      <h2>Ejercicio 2: Lista de Estudiantes</h2>
      
      <input 
        type="text" 
        v-model="searchQuery" 
        placeholder="Buscar por nombre..."
      />
      
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Apellido</th>
            <th>Calificación</th>
          </tr>
        </thead>
        <tbody>
          <tr 
            v-for="student in filteredStudents" 
            :key="student.id"
            :class="{ 'failing': student.grade < 70 }"
          >
            <td>{{ student.id }}</td>
            <td>{{ student.firstName }}</td>
            <td>{{ student.lastName }}</td>
            <td>{{ student.grade }}</td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<style scoped>
section {
  margin: 20px 0;
}

hr {
  margin: 40px 0;
  border: 1px solid #ccc;
}

/* Ejercicio 1: Foco */
img {
  display: block;
  margin-bottom: 10px;
}

/* Ejercicio 2: Tabla */
.failing {
  background-color: red;
  color: white;
}

table {
  border-collapse: collapse;
  width: 100%;
  margin-top: 20px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

input {
  padding: 8px;
  margin: 10px 0;
  width: 300px;
}
</style>
