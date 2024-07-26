<script setup>
import { ref, reactive, watch, onMounted } from "vue";
import { uid } from "uid";
import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import Empleado from "./components/Empleado.vue";

const empleados = ref([]);
const empleado = reactive({
  id: null,
  nombre: "",
  apellido: "",
  serie: "",
  fecha: new Date().toISOString().split("T")[0],
  ingreso: "",
  salida: "",
});

watch(
  empleados,
  () => {
    guardarLocal();
  },
  {
    deep: true,
  }
);

const guardarLocal = () => {
  localStorage.setItem("empleados", JSON.stringify(empleados.value));
};
onMounted(() => {
  const empleadosStorage = localStorage.getItem("empleados");
  if (empleadosStorage) {
    empleados.value = JSON.parse(empleadosStorage);
  }
});

const registrarEmpleado = (resolve) => {
  let resultado = false;
  if (empleado.id) {
    const { id } = empleado;
    const i = empleados.value.findIndex((e) => e.id === id);
    empleados.value[i] = { ...empleado };
    limpiarFormulario();
    resultado = true;
  } else {
    if (!empleadoExiste(empleado.serie)) {
      empleados.value.push({
        ...empleado,
        id: uid(),
      });
      limpiarFormulario();
      resultado = true;
    } else {
      alert("El empleado ya ha sido dado de alta");
    }
  }
  resolve(resultado);
};

const limpiarFormulario = () => {
  empleado.id = null;
  empleado.nombre = "";
  empleado.apellido = "";
  empleado.serie = "";
  empleado.fecha = new Date().toISOString().split("T")[0];
  empleado.ingreso = "";
  empleado.salida = "";
};

const editarEmpleado = (id) => {
  const empleadoEditar = empleados.value.filter((e) => e.id === id)[0];
  Object.assign(empleado, empleadoEditar);
};

const eliminarEmpleado = (id) => {
  const resultado = confirm("Desea Eliminar a este Empleado?");
  if (resultado) {
    empleados.value = empleados.value.filter((e) => e.id !== id);
  }
};

const empleadoExiste = (serie) => {
  return empleados.value.some((e) => e.serie === serie);
};
</script>

<template>
  <div class="w-[95%] mx-auto mt-5">
    <Header />
    <main class="mt-10 md:flex">
      <Formulario
        v-model:nombre="empleado.nombre"
        v-model:apellido="empleado.apellido"
        v-model:serie="empleado.serie"
        v-model:fecha="empleado.fecha"
        v-model:ingreso="empleado.ingreso"
        v-model:salida="empleado.salida"
        @registrar-empleado="registrarEmpleado"
        :id="empleado.id"
      />
      <div class="md:w-1/2 md:h-screen">
        <h3 class="text-center text-white font-black text-3xl">
          Administra a tus Empleados
        </h3>
        <div v-if="empleados.length > 0">
          <p class="text-lg text-white mt-5 mb-5 text-center">
            Informacion de
            <span class="text-cyan-600 font-bold">los Empleados</span>
          </p>
          <div class="md:h-screen overflow-y-scroll">
            <Empleado
              v-for="empleado in empleados"
              :empleado="empleado"
              :key="empleado.id"
              @editar-empleado="editarEmpleado"
              @eliminar-empleado="eliminarEmpleado"
            />
          </div>
        </div>
        <p v-else class="mt-5 text-2xl text-center text-gray-400">
          Sin Empleados Registrados
        </p>
      </div>
    </main>
  </div>
</template>
