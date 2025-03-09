<template>
  <TransitionRoot as="template" :show="isOpen">
    <Dialog class="relative z-10" @close="closeModal">
      <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100"
        leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500/75 transition-opacity" />
      </TransitionChild>
      <div class="fixed inset-0 z-10 w-screen overflow-y-auto">
        <div class="flex min-h-full items-center justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild as="template" enter="ease-out duration-300"
            enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200"
            leave-from="opacity-100 translate-y-0 sm:scale-100"
            leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
            <DialogPanel
              class="relative transform rounded-lg bg-white px-4 pt-5 pb-4 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-4xl sm:p-6">
                <button type="button" class="rounded-md text-gray-400 hover:text-gray-500 focus:outline-none absolute -right-3 -top-3 bg-white shadow-lg rounded-lg p-2"
                  @click="closeModal">
                  <span class="sr-only">Close</span>
                  <XMarkIcon class="h-6 w-6" aria-hidden="true" />
                </button>
              <div class="divide-y divide-gray-400 ">
                <!-- Parte superior: Nome e avatar -->
                <div class="text-center pb-6"> <!-- Adicionei pb-6 para aumentar o espaçamento -->
                  <img class="inline-block size-40 rounded-full" :src="user?.avatar_url" alt="" />
                  <DialogTitle as="h3" class="text-2xl font-semibold leading-6 text-gray-900 mt-4">{{ user.name }}
                  </DialogTitle>
                  <p>{{ user.login }}</p>
                </div>
                <!-- Parte inferior: URL e ID -->
                <div class="pt-6"> <!-- Adicionei pt-6 para aumentar o espaçamento -->
                  <p class="text-lg text-gray-500">URL do perfil: <a :href="user.profileUrl"
                      class="text-blue-500 hover:text-blue-700">{{ user.profileUrl }}</a></p>
                  <p class="text-lg text-gray-500">ID: {{ user.id }}</p>
                </div>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue';
import { XMarkIcon } from '@heroicons/vue/24/outline';

// Props do modal
const props = defineProps({
  isOpen: {
    type: Boolean,
    required: true,
  },
  user: {
    type: Object,
    required: true,
    default: () => ({
      name: '',
      profileUrl: '',
      id: '',
      avatar_url: '',
    }),
  },
});

// Emit para fechar o modal --- esse é o meu código
const emit = defineEmits(['close']);

// Estado local para controlar a abertura do modal
const isOpen = ref(props.isOpen);

// Observar mudanças na prop `isOpen`
watch(() => props.isOpen, (newVal) => {
  isOpen.value = newVal;
});

// Fechar o modal e emitir o evento
const closeModal = () => {
  isOpen.value = false;
  emit('close');
};
</script>