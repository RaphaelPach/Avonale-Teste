<script setup lang="ts">
import { EnvelopeIcon, PhoneIcon } from '@heroicons/vue/20/solid';

const people = [
  {
    name: 'Jane Cooper',
    title: 'Paradigm Representative',
    role: 'Admin',
    email: 'janecooper@example.com',
    telephone: '+1-202-555-0170',
    imageUrl:
      'https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=4&w=256&h=256&q=60',
  },
];
</script>

<template>
    <ul role="list" class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
      <li v-for="person in people" :key="person.email" class="col-span-1 w-[240px] flex flex-col divide-y divide-gray-200 rounded-lg bg-white text-center shadow-lg">
        <div class="flex flex-1 flex-col">
          <img class="mx-auto h-40 w-full flex-shrink-0" :src="person.imageUrl" alt="" />
          <div class="flex flex-row justify-between p-3">
            <h3 class="text-lg font-semibold text-gray-900">{{ person.name }}</h3>
            <span>icon</span>
          </div>
          <dl class="mt-1">
            <dd class="text-sm text-gray-500 text-left p-3">{{ person.title }}</dd>
          </dl>
        </div>
        <div>
          <div class="-mt-px flex divide-x divide-gray-200">
            <div class="flex w-0 flex-1">
              <a :href="`mailto:${person.email}`" class="relative -mr-px inline-flex w-0 flex-1 items-center justify-center gap-x-3 rounded-bl-lg border border-transparent py-4 text-sm font-semibold text-gray-900 hover:bg-gray-50">
                <EnvelopeIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
                Email
              </a>
            </div>
            <div class="-ml-px flex w-0 flex-1">
              <a :href="`tel:${person.telephone}`" class="relative inline-flex w-0 flex-1 items-center justify-center gap-x-3 rounded-br-lg border border-transparent py-4 text-sm font-semibold text-gray-900 hover:bg-gray-50">
                <PhoneIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
                Call
              </a>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </template>