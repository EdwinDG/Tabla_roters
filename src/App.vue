<template>
  <div>
    <q-table title="Buses" :rows="rows" :columns="columns" row-key="name">
    <template v-slot:body-cell-botones="props">
      <q-td :props="props">
        <q-btn color="primary" label="Editar" @click="EditarBus(props.row._id)" />
        <q-btn color="negative" label="Borrar" @click="borrarBus(props.row._id)" />
        <q-btn color="amber" glossy :label="habilitar" @click="InactivarBus(props.row._id)" />
      </q-td>
    </template>
  </q-table>
  </div>
  
</template>

<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';

let apiUrl = 'https://transporte-czaa.onrender.com/api/bus/buses';
let buses = ref([]);
let rows = ref([]);

let fixed = ref(false)

let habilitar = ref('Inactivar')

async function obtenerInfo() {
  try {
    const responseBuses = await axios.get(apiUrl);
    buses.value = responseBuses.data.buses; 
    rows.value = responseBuses.data.buses; 
  } catch (error) {
    console.error('Error al obtener la información de los buses:', error);
  }
}

const columns = [
  { name: 'placa', label: 'Placa', field: 'placa', sortable: true },
  { name: 'numero_bus', label: 'Número de Bus', field: 'numero_bus', sortable: true },
  { name: 'cantidad_asientos', label: 'Cantidad de Asientos', field: 'cantidad_asientos' },
  { name: 'empresa_asignada', label: 'Empresa Asignada', field: 'empresa_asignada' },
  { name: 'estado', label: 'Estado', field: 'estado', format: (val) => (val ? 'Activo' : 'Inactivo') },
  { name: 'createAT', label: 'Fecha de Creación', field: 'createAT', sortable: true,},
  { name: 'botones', label: 'Opciones',
    field: row => null, 
    format: (val, row) => {
      return `
        <q-btn color="primary" label="Editar" @click="editarBus('${row._id}')" />
        <q-btn color="negative" label="Borrar" @click="borrarBus('${row._id}')" />
        <q-btn color="amber" glossy :label="Inactivar"  @click="InactivarBus('${row._id}'/>`;
    },
    "sortable": false, 
  },
];
onMounted(()=>{
  obtenerInfo()
})
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