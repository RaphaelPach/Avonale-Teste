<script setup lang="ts">
import SearchBar from './components/SearchBar.vue';
import Cards from './components/Cards.vue';


const handleSearch = (username: string) => {
  console.log('Buscando repositórios de:', username);
  // Aqui vamos adicionar a lógica para buscar na API do GitHub em breve
};
</script>

<template>
  <div class="min-h-screen bg-[#f5f5f5]">
    <div class="flex flex-col p-6 gap-10 max-w-7xl mx-auto">
      <SearchBar @search="handleSearch" />
      <Cards />
    </div>
  </div>
</template>
