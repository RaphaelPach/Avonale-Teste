<script setup lang="ts">
//import { EnvelopeIcon, PhoneIcon } from '@heroicons/vue/20/solid';
import api from '../Services/Api';
import { ref } from 'vue';

type Repository = {
  name: string;
  description: string;
};

type Owner = {
  avatar_url: string;
  starred_url: string;
  name: string;
  html_url: string;
};


const refOwner = ref<Owner>();
const repositories = ref<Repository[]>([
]);

const response: any = async () => {
  const repos = await api.get('/users/octocat/repos');
  repositories.value = repos.data.map((repo: Repository) => ({
    name: repo.name,
    description: repo.description,
  }),
  );

  const repoOwner: any = async () => {
    const repo = await api.get('/users/octocat');
    refOwner.value = repo.data
  }
  repoOwner();
}


response();

</script>

<template>
  <ul role="list" class="grid grid-cols-4 gap-10 w-full w-full place-items-center">
    <li v-for="repo in repositories" :key="repo.name"
      class="col-span-1 w-full h-[350px] flex flex-col divide-y divide-gray-200 rounded-lg bg-white text-center shadow-lg">
      <div class="flex flex-1 flex-col">
        <img class="mx-auto h-40 w-full flex-shrink-0" :src="refOwner?.avatar_url" alt="" />
        <div class="flex flex-row justify-between p-3">
          <h3 class="text-lg font-semibold text-gray-900">{{ repo.name }}</h3>
          <span>icon</span>
        </div>
        <dl class="mt-1">
          <dd class="text-sm text-gray-500 text-left p-3">{{ repo.description }}</dd>
        </dl>
      </div>
      <div>
        <div class="-mt-px flex">
          <div class="flex w-0 flex-1">
            <a :href="refOwner?.html_url" target="_blank" class="p-3 text-sm font-semibold text-gray-900">
              {{ refOwner?.name }}
            </a>
          </div>
          <!--<div class="-ml-px flex w-0 flex-1">
              <a :href="`tel:${person.telephone}`" class="relative inline-flex w-0 flex-1 items-center justify-center gap-x-3 rounded-br-lg border border-transparent py-4 text-sm font-semibold text-gray-900 hover:bg-gray-50">
                <PhoneIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
                Call
              </a>
            </div>-->
        </div>
      </div>
    </li>
  </ul>
</template>