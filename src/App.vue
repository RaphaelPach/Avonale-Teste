<script setup lang="ts">
import SearchBar from './components/SearchBar.vue';
import Cards from './components/Cards.vue';

const handleSearch = (username: string) => {
  console.log('Buscando repositórios de:', username);
  // Aqui vamos adicionar a lógica para buscar na API do GitHub em breve
};
</script>

<template>
  <div class="min-h-screen bg-[#f5f5f5] flex flex-col p-6 gap-10">
    <SearchBar @search="handleSearch" />
    <Cards />
  </div>
</template>
