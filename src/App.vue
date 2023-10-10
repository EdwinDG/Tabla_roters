<template>
  <div class="body">

    <div class="tabladiv">
      <div class="q-pa-md">
        <q-markup-table dark class="bg-white">
          <thead class="bg-primary">
            <tr class="text-dark1">
              <th colspan="3">DATOS TICKETS</th>
            </tr>
            <tr class="text-dark">
              <th class="text-center">cedula</th>
              <th class="text-center">nombre</th>
              <th class="text-center">telefono</th>
              <th class="text-center">Acciones</th>
            </tr>
          </thead>
          <tbody class="text-dark">
            <tr v-for="cliente in DatosData" :key="cliente">
              <td class="text-center">{{ cliente.cedula }}</td>
              <td class="text-center">{{ cliente.nombre }}</td>
              <td class="text-center">{{ cliente.telefono }}</td>
              <td class="text-center">
                <q-btn class="ttp" icon="edit" color="primary"></q-btn>
                <q-btn class="ttp" icon="delete" color="negative"></q-btn>
              </td>
            </tr>
          </tbody>
        </q-markup-table>
        <div class="text-right">
          <button class="btn" @click="mostrarModal">Agregar</button>
        </div>

        <q-dialog v-model="mostrarModalAgregar">
          <q-card>
            <q-card-section>
              <h4>Agregar nuevo elemento</h4>
              <div class="form-group">
                <div class="input-group">
                  <label for="campo1">Campo 1:</label>
                  <input id="campo1" class="datos" type="text">
                </div>
                <div class="input-group">
                  <label for="campo2">Campo 2:</label>
                  <input id="campo2" class="datos" type="text">
                </div>
                <div class="input-group">
                  <label for="campo3">Campo 3:</label>
                  <input id="campo3" class="datos" type="text">
                </div>
                <div class="input-group">
                  <label for="campo4">Campo 4:</label>
                  <input id="campo4" class="datos" type="text">
                </div>
              </div>
            </q-card-section>
            <q-card-actions align="right">
              <q-btn label="Guardar" color="primary" @click="guardarNuevoElemento" />
              <q-btn label="Cancelar" color="negative" @click="cerrarModal" />
            </q-card-actions>
          </q-card>
        </q-dialog>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";

let DatosData = ref([]);
let mostrarModalAgregar = ref(false);

function mostrarModal() {
  mostrarModalAgregar.value = true;
}

function cerrarModal() {
  mostrarModalAgregar.value = false;
}

async function ObtenerDatos() {
  const response = await axios.get(
    `https://transporte2.onrender.com/api/cliente/cliente`
  );
  const data = response.data;

  data.cliente.forEach((cliente) => {
    DatosData.value.push({
      Id: cliente._id,
      cedula: cliente.cedula,
      nombre: cliente.nombre,
      telefono: cliente.telefono,
    });
  });

  console.log(data);
}

onMounted(() => {
  ObtenerDatos();
});
</script>


<style scoped>
.tabladiv {
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.text-center {
  border: 1px solid black;
}

.text-dark1 {
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid black;
}

.ttp {
  margin: 5px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.input-group {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

label {
  margin-right: 20px;
}

.datos {
  flex: 1;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.btn {
  margin-top: 10px;
}
</style>