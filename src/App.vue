<template>
  <div class="container">
    <h1>Gestión de Huéspedes 🏨</h1>

    <!-- Formulario para agregar huésped -->
    <form @submit.prevent="agregarHuesped" class="formulario">
      <input v-model="nuevo.nombre" placeholder="Nombre" required />
      <input v-model="nuevo.apellido" placeholder="Apellido" required />
      <button type="submit">Agregar huésped</button>
    </form>

    <!-- Lista de huéspedes -->
    <h2>Lista de huéspedes:</h2>
    <ul v-if="huespedes.length > 0">
      <li v-for="h in huespedes" :key="h.id">
        <strong>{{ h.nombre }} {{ h.apellido }}</strong>
      </li>
    </ul>
    <p v-else>No hay huéspedes registrados aún.</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      huespedes: [],
      nuevo: {
        nombre: "",
        apellido: ""
      }
    };
  },
  async created() {
    await this.obtenerHuespedes();
  },
  methods: {
    // Obtener todos los huéspedes
    async obtenerHuespedes() {
      try {
        const res = await axios.get("http://localhost:4001/huespedes");
        this.huespedes = res.data;
      } catch (error) {
        console.error("❌ Error al obtener huéspedes:", error);
      }
    },
    // Agregar nuevo huésped
    async agregarHuesped() {
      try {
        await axios.post("http://localhost:4001/huespedes", this.nuevo);
        this.nuevo = { nombre: "", apellido: "" }; // limpia los campos
        await this.obtenerHuespedes(); // refresca la lista
      } catch (error) {
        console.error("❌ Error al agregar huésped:", error);
      }
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  text-align: center;
  font-family: Arial, sans-serif;
}

h1 {
  color: #42b983;
}

.formulario {
  margin-bottom: 30px;
}

input {
  margin: 5px;
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #36976e;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin: 8px 0;
  padding: 8px;
  border-bottom: 1px solid #ddd;
}
</style>
