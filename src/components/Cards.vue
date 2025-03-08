<script setup lang="ts">
import { UsersIcon, LockClosedIcon } from '@heroicons/vue/24/solid';
import api from '../Services/Api';
import { ref, computed } from 'vue';
import SearchBar from './SearchBar.vue'; // Importe o componente SearchBar
import UserModal from './Modal.vue'; // Importe o componente UserModal

type Repository = {
  name: string;
  description: string;
  html_url: string;
  private: boolean;
  created_at?: Date;
};

type Owner = {
  id: string;
  avatar_url: string;
  starred_url: string;
  name: string;
  html_url: string;
};

const refOwner = ref<Owner>();
const repositories = ref<Repository[]>([]);
const search = ref<string>(''); // Estado para o termo de busca
const isModalOpen = ref(false); // Estado para controlar a abertura do modal

// Computed property para filtrar repositórios
const filteredRepositories = computed(() => {
  return repositories.value.filter((repo) => {
    return repo.name.toLowerCase().includes(search.value.toLowerCase());
  });
});

// Função para capturar o evento de busca do SearchBar
const handleSearch = (term: string) => {
  search.value = term; // Atualiza o estado de busca
};

// Função para abrir o modal
const openModal = () => {
  isModalOpen.value = true;
};

// Função para fechar o modal
const closeModal = () => {
  isModalOpen.value = false;
};

const response: any = async () => {
  const repos = await api.get('/users/octocat/repos');
  repositories.value = repos.data.map((repo: Repository) => ({
    name: repo.name,
    description: repo.description,
    html_url: repo.html_url,
    private: repo.private,
    created_at: repo.created_at ? new Date(repo.created_at) : null,
  }));

  const repoOwner: any = async () => {
    const repo = await api.get('/users/octocat');
    refOwner.value = repo.data;
  };
  repoOwner();
};

response();
</script>

<template>
  <div class="flex flex-col gap-6">
    <SearchBar @search="handleSearch" />
    <ul role="list" class="grid grid-cols-4 gap-10 w-full place-items-center items-start">
      <li v-for="repo in filteredRepositories" :key="repo.name"
        class="col-span-1 w-full min-h-[350px] flex flex-col rounded-lg bg-white text-center shadow-lg">
        <div class="flex flex-1 flex-col">
          <img class="mx-auto h-40 w-full flex-shrink-0" :src="refOwner?.avatar_url" alt="" />
          <div class="flex flex-row justify-between p-3">
            <a :href="repo?.html_url" target="_blank" class="text-lg font-semibold text-gray-900">{{ repo.name }}</a>
            <span v-if="repo.private">
              <LockClosedIcon class="h-6 w-6 text-gray-500" />
            </span>
            <span v-else>
              <UsersIcon class="h-6 w-6 text-gray-500" />
            </span>
          </div>
          <!-- Descrição com reticências -->
          <dl class="mt-1">
            <dd class="text-sm text-gray-500 text-left p-3 truncate">{{ repo.description }}</dd>
          </dl>
        </div>
        <div>
          <div class="flex flex-row items-center p-3">
            <div>
              <img class="inline-block size-12 rounded-full" :src="refOwner?.avatar_url" alt="" />
            </div>
            <div class="w-[80%]">
              <p class="p-3 text-base font-semibold text-gray-900 text-center w-full cursor-pointer" @click="openModal">
                {{ refOwner?.name }}
              </p>
              <p class="text-sm text-gray-500">{{ repo.created_at?.toLocaleDateString("pt-BR") }}</p>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>

  <!-- Modal do usuário -->
  <UserModal :isOpen="isModalOpen" :user="{ name: refOwner?.name, profileUrl: refOwner?.html_url, id: refOwner?.id, avatar_url: refOwner?.avatar_url }" @close="closeModal"  />
</template>

<style scoped>
/* Adicione este estilo para truncar o texto da descrição */
.truncate {
  white-space: nowrap;
  /* Impede que o texto quebre em várias linhas */
  overflow: hidden;
  /* Esconde o texto que ultrapassar o contêiner */
  text-overflow: ellipsis;
  /* Adiciona as reticências (...) */
}
</style>