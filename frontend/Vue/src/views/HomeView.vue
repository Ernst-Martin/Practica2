<script setup>
import { ref } from "vue";
import axios from "axios"; // Asegúrate de instalar axios: npm install axios

// Opciones de almacenamiento
const storageOptions = ["Class Storage", "JSON", "CSV"];
const selectedStorage = ref("Class Storage");

// Campos de entrada
const filename = ref("");
const inputData = ref("");

// Respuesta inicial (vacía)
const response = ref({});

// Función para obtener archivos (GET /api/hello)
const getFiles = async () => {
  try {
    const res = await axios.get("http://localhost:8000/api/hello"); // Llama a la API REST
    response.value = res.data; // Actualiza la respuesta con los datos obtenidos
  } catch (error) {
    console.error("Error obteniendo archivos:", error);
    response.value = { error: "No se pudo obtener la lista de archivos." };
  }
};
</script>

<template>
  <main>
    <header>
      <h1>PR UD 2</h1>
    </header>
    <section>
      <aside>
        <ul>
          <li
            v-for="option in storageOptions"
            :key="option"
            :class="{ active: selectedStorage === option }"
            @click="selectedStorage = option"
          >
            {{ option }}
          </li>
        </ul>
      </aside>
      <div>
        <div class="buttons">
          <button @click="getFiles">Get Files</button>
          <button>Store</button>
          <button>Show</button>
          <button>Update</button>
          <button>Delete</button>
        </div>
        <textarea readonly>{{ JSON.stringify(response, null, 2) }}</textarea>
        <button>Send</button>
      </div>
    </section>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
  padding: 1rem;
}

header h1 {
  margin-bottom: 1rem;
}

section {
  display: flex;
  gap: 1rem;
}

aside {
  border: 1px solid #ddd;
  padding: 1rem;
}

aside ul {
  list-style: none;
  padding: 0;
}

aside li {
  padding: 0.5rem;
  cursor: pointer;
}

aside li.active {
  font-weight: bold;
  color: white;
  background-color: #007bff;
}

.buttons {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 1rem;
}

button {
  padding: 0.5rem 1rem;
  border: 1px solid #007bff;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  border-radius: 4px;
}

button:hover {
  background-color: #0056b3;
}
</style>
