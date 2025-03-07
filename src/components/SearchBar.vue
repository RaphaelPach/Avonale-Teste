<script setup lang="ts">
import { MagnifyingGlassIcon } from '@heroicons/vue/24/solid'; 
import { ref } from 'vue';


const username = ref('');
const emit = defineEmits(['search']);

const handleSearch = () => {
  if (username.value.trim() !== '') {
    emit('search', username.value);
  }
};
</script>

<template>
  <div class="flex flex-row h-16 items-center w-full p-4 shadow-md bg-white rounded-b-lg">
    <MagnifyingGlassIcon class="size-6 text-gray-500" />
    <input
      v-model="username"
      type="text"
      placeholder="Buscar repositórios"
      class="p-3 w-full outline-none"
      @keyup.enter="handleSearch"
    />
  </div>
</template>
