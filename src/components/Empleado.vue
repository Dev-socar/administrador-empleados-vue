<script setup>
const props = defineProps({
  empleado: {
    type: Object,
    required: true,
  },
});

defineEmits(['editar-empleado','eliminar-empleado'])
</script>

<template>
  <div
    class="mx-5 mb-5 space-y-3 bg-gray-800 p-5 rounded-lg shadow-sm shadow-slate-600"
  >
    <p class="text-lg font-semibold text-cyan-500">
      Nombre: <span class="text-white">{{ empleado.nombre }}</span>
    </p>
    <p class="text-lg font-semibold text-cyan-500">
      Apellido: <span class="text-white">{{ empleado.apellido }}</span>
    </p>
    <p class="text-lg font-semibold text-cyan-500">
      # Serie: <span class="text-white">{{ empleado.serie }}</span>
    </p>
    <p class="text-lg font-semibold text-cyan-500">
      Fecha: <span class="text-white">{{ empleado.fecha }}</span>
    </p>
    <p class="text-lg font-semibold text-cyan-500">
      Hora de Entrada: <span class="text-white">{{ empleado.ingreso }}</span>
    </p>
    <p class="text-lg font-semibold text-cyan-500">
      Hora de Salida: <span class="text-white">{{ empleado.salida }}</span>
    </p>
    <div class="flex gap-5">
      <button
        type="button"
        class="block w-full py-2 px-10 bg-cyan-500 hover:bg-cyan-700 transition-colors text-white font-bold uppercase rounded-lg"
        @click="$emit('editar-empleado', empleado.id)"
      >
        Editar
      </button>
      <button
        type="button"
        class="block w-full py-2 px-10 border border-gray-500 hover:bg-gray-700 transition-colors text-white font-bold uppercase rounded-lg"
         @click="$emit('eliminar-empleado', empleado.id)"
      >
        Eliminar
      </button>
    </div>
  </div>
</template>
