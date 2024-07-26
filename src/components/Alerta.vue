<script setup>
import { computed } from "vue";

const props = defineProps({
  alerta: {
    type: Object,
    required: true,
  },
});

const isError = computed(() => {
  return props.alerta.tipo === "error";
});
</script>

<template>
  <div
    :class="[isError ? 'bg-red-500' : ' bg-green-500']"
    class="text-white text-center p-2 font-bold uppercase rounded-lg mb-3"
  >
    {{ alerta.msg }}
  </div>
</template>
