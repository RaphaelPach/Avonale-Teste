<script setup lang="ts">
import { MagnifyingGlassIcon } from '@heroicons/vue/24/solid'; 
import { ref, watch } from 'vue';

const username = ref('');
const emit = defineEmits(['search']);

// Emitir o evento de busca sempre que o valor de username mudar
watch(username, (newValue) => {
  emit('search', newValue);
});
</script>

<template>
  <div class="flex flex-row h-16 items-center w-full p-4 shadow-md bg-white rounded-b-lg">
    <MagnifyingGlassIcon class="size-6 text-gray-500" />
    <input
      v-model="username"
      type="text"
      placeholder="Buscar repositórios"
      class="p-3 w-full outline-none"
    />
  </div>
</template>