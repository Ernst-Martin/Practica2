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

// Función para almacenar un archivo (POST /api/hello)
const storeFile = async () => {
  if (!filename.value.trim() || !inputData.value.trim()) {
    response.value = { error: "El nombre del archivo y el contenido son obligatorios." };
    return;
  }

  try {
    const res = await axios.post("http://localhost:8000/api/hello", {
      filename: filename.value,
      content: inputData.value,
    }); // Llama a la API REST con el nombre del archivo y su contenido

    response.value = res.data; // Actualiza la respuesta con los datos obtenidos
  } catch (error) {
    console.error("Error almacenando archivo:", error);
    response.value = { error: "No se pudo almacenar el archivo." };
  }
};

// Función para mostrar el contenido de un archivo (GET /api/hello/{filename})
const showFile = async () => {
  if (!filename.value.trim()) {
    response.value = { error: "El nombre del archivo es obligatorio." };
    return;
  }

  try {
    const res = await axios.get(`http://localhost:8000/api/hello/${filename.value}`); // Llama a la API REST
    response.value = res.data; // Actualiza la respuesta con los datos obtenidos
  } catch (error) {
    console.error("Error mostrando archivo:", error);
    response.value = { error: "No se pudo mostrar el archivo." };
  }
};

// Función para actualizar un archivo (PUT /api/hello/{filename})
const updateFile = async () => {
  if (!filename.value.trim() || !inputData.value.trim()) {
    response.value = { error: "El nombre del archivo y el contenido son obligatorios para actualizar." };
    return;
  }

  try {
    const res = await axios.put(`http://localhost:8000/api/hello/${filename.value}`, {
      content: inputData.value,
    }); // Llama a la API REST para actualizar el archivo

    response.value = res.data; // Actualiza la respuesta con los datos obtenidos
  } catch (error) {
    console.error("Error actualizando archivo:", error);
    response.value = { error: "No se pudo actualizar el archivo." };
  }
};

// Función para eliminar un archivo (DELETE /api/hello/{filename})
const deleteFile = async () => {
  if (!filename.value.trim()) {
    response.value = { error: "El nombre del archivo es obligatorio para eliminar." };
    return;
  }

  try {
    const res = await axios.delete(`http://localhost:8000/api/hello/${filename.value}`); // Llama a la API REST para eliminar el archivo
    response.value = res.data; // Actualiza la respuesta con los datos obtenidos
  } catch (error) {
    console.error("Error eliminando archivo:", error);
    response.value = { error: "No se pudo eliminar el archivo." };
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
          <button @click="storeFile">Store</button>
          <button @click="showFile">Show</button>
          <button @click="updateFile">Update</button>
          <button @click="deleteFile">Delete</button>
        </div>
        <textarea
          placeholder="Respuesta del servidor"
          readonly
        >{{ JSON.stringify(response, null, 2) }}</textarea>
        <input
          type="text"
          v-model="filename"
          placeholder="Nombre del archivo"
        />
        <textarea
          v-model="inputData"
          placeholder="Contenido del archivo"
        ></textarea>
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

input[type="text"] {
  width: 100%;
  padding: 0.5rem;
  margin-bottom: 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
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
