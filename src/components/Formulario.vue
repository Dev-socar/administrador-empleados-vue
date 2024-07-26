<script setup>
import { reactive, computed } from "vue";
import Alerta from "./Alerta.vue";

const alerta = reactive({
  msg: "",
  tipo: "",
});

const emit = defineEmits([
  "update:nombre",
  "update:apellido",
  "update:serie",
  "update:fecha",
  "update:ingreso",
  "update:salida",
  "registrar-empleado",
]);

const props = defineProps({
  id: {
    type: [String, null],
    required: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  apellido: {
    type: String,
    required: true,
  },
  serie: {
    type: [Number, String],
    required: true,
  },
  fecha: {
    type: String,
    required: true,
  },
  ingreso: {
    type: String,
    required: true,
  },
  salida: {
    type: String,
    required: true,
  },
});

const handleSubmit = async () => {
  if (Object.values(props).includes("")) {
    alerta.msg = "Todos los Campos son Obligatorios";
    alerta.tipo = "error";
    return;
  }

  const resultado = await new Promise((resolve) => {
    emit("registrar-empleado", resolve);
  });

  if (resultado) {
    alerta.msg = "Empleado Registrado Correctamente";
    alerta.tipo = "exito";
    setTimeout(() => {
      alerta.msg = "";
      alerta.tipo = "";
    }, 3000);
  }
};

const editando = computed(() => {
  return props.id;
});
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="text-white text-center text-3xl">Control de Asistencia</h2>
    <p class="text-lg mt-5 text-center mb-5 text-white">
      Agrega Empleados y
      <span class="text-cyan-500 font-bold">Administralos</span>
    </p>
    <Alerta v-if="alerta.msg" :alerta="alerta" />
    <form
      class="bg-slate-800 py-5 px-3 rounded-lg shadow-sm shadow-slate-600 space-y-5"
      @submit.prevent="handleSubmit()"
    >
      <div class="space-y-2">
        <label for="nombre" class="text-lg font-semibold text-white block"
          >Nombre</label
        >
        <input
          type="text"
          id="nombre"
          class="w-full p-2 rounded-md bg-slate-900 placeholder-gray-400 text-gray-400"
          placeholder="Ingresa el Nombre del Empleado"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
        />
      </div>
      <div class="space-y-2">
        <label for="apellido" class="text-lg font-semibold text-white block"
          >Apellido</label
        >
        <input
          type="text"
          id="apellido"
          class="w-full p-2 rounded-md bg-slate-900 placeholder-gray-400 text-gray-400"
          placeholder="Ingresa el Apellido del Empleado"
          :value="apellido"
          @input="$emit('update:apellido', $event.target.value)"
        />
      </div>
      <div class="space-y-2">
        <label for="serie" class="text-lg font-semibold text-white block"
          >Numero de Serie</label
        >
        <input
          type="number"
          id="serie"
          class="w-full p-2 rounded-md bg-slate-900 placeholder-gray-400 text-gray-400"
          placeholder="Ingresa el numero de Serie del Empleado"
          :value="serie"
          @input="$emit('update:serie', $event.target.value)"
        />
      </div>
      <div class="space-y-2">
        <label for="fecha" class="text-lg font-semibold text-white block"
          >Fecha</label
        >
        <input
          type="date"
          id="fecha"
          class="w-full p-2 rounded-md bg-slate-900 text-gray-400"
          :value="fecha"
          @input="$emit('update:fecha', $event.target.value)"
        />
      </div>
      <div class="space-y-2">
        <label for="ingreso" class="text-lg font-semibold text-white block"
          >Hora de Entrada</label
        >
        <input
          type="time"
          id="ingreso"
          class="w-full p-2 rounded-md bg-slate-900 text-gray-400"
          :value="ingreso"
          @input="$emit('update:ingreso', $event.target.value)"
        />
      </div>
      <div class="space-y-2">
        <label for="salida" class="text-lg font-semibold text-white block"
          >Hora de Salida</label
        >
        <input
          type="time"
          id="salida"
          class="w-full p-2 rounded-md bg-slate-900 text-gray-400"
          :value="salida"
          @input="$emit('update:salida', $event.target.value)"
        />
      </div>
      <input
        type="submit"
        class="w-full p-3 bg-sky-500 hover:bg-sky-600 transition-colors text-white text-lg uppercase font-semibold rounded-md cursor-pointer"
        :value="[editando ? 'Guardar Cambios' : ' Registrar Empleado']"
      />
    </form>
  </div>
</template>
