<template>
  <div class="body">
    <div class="container">
      <button style="margin: 20px 0px" id="boton">Agregar cliente</button>
    </div>

    <div class="tabladiv">
      
        <div class="q-pa-md">
          <q-markup-table dark class="bg-white " >
            <thead class="bg-primary" >
              <tr class="text-dark">
             
                <th class="text-right ">cedula</th>
                <th class="text-right">nombre</th>
                <th class="text-right">telefono</th>
               
              </tr>
            </thead>
            <tbody class="text-dark">
              <tr v-for="cliente in DatosData" :key="cliente">

                <td class="text-right">{{cliente.cedula}}</td>
                <td class="text-right">{{cliente.nombre}}</td>
                <td class="text-right">{{cliente.telefono}}</td>
              </tr>
              
            </tbody>
          </q-markup-table>
        </div>
      
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
const Codigo = ref("");
const Nombre = ref("");
const Cantidad = ref("");
const Precio = ref("");
const Costo = ref("");

let codigodivRef = ref();
let nombredivRef = ref();
let cantidaddivRef = ref();
let preciodivRef = ref();
let costodivRef = ref();

let modalbody = ref("");
let Err = ref("Datos de la tabla");
let DatosData = ref([]);
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


function formatoHora(hora) {
  const fechaHora = new Date(hora);
  const opcionesDeFormato = {
    hour: "2-digit",
    minute: "2-digit",
    second: "2-digit",
  };

  return fechaHora.toLocaleTimeString("es-ES", opcionesDeFormato);
}
function formatoFecha(fecha) {
  const fechaHora = new Date(fecha);
  const opcionesDeFormato = {
    year: "numeric",
    month: "long",
    day: "numeric",
  };

  return fechaHora.toLocaleDateString("es-ES", opcionesDeFormato);
}
onMounted(() => {
  ObtenerDatos();
});
</script>


<style scoped>

</style>