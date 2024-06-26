<script setup>
import { ref } from 'vue';
import { useRoute } from 'vue-router';
import { useQuery } from '@vue/apollo-composable';
import gql from 'graphql-tag';

const route = useRoute();
const { id } = route.params;
const LOCATION_DETAILS_QUERY = gql`
  query GetLocationDetails($id: ID!) {
    location(id: $id) {
      name
      type
      dimension
      residents {
        image
        id
        name
      }
    }
  }
`;

const { result, loading, error } = useQuery(LOCATION_DETAILS_QUERY, { id });

const isMenuOpen = ref(false);

function toggleMenu() {
  isMenuOpen.value = !isMenuOpen.value;
}
</script>
<template>
  <div class="bg-[#3b2d2d] text-white min-h-screen flex flex-col">
    <nav class="sticky top-0 z-50 bg-[#3b2d2d] px-4 py-2 flex justify-between items-center border-b border-gray-700">
      <button @click="toggleMenu" class="text-white bg-transparent rounded-full border border-red-500 px-2 py-2 hover:bg-red-500 transition-colors">
        <svg class="w-6 h-6 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
        </svg>
      </button>
      <ul class="flex space-x-2 text-sm">
        <li> <router-link :to="'/' " class="rounded-full px-3 py-2 border border-red-500 flex items-center justify-center text-white hover:bg-red-500 transition-colors">Home</router-link></li>
        <li> <router-link :to="'/Episodes' " class="rounded-full px-3 py-2 border border-red-500 flex items-center justify-center text-white hover:bg-red-500 transition-colors"> Episodes</router-link></li>
        <li> <router-link :to="'/characters' " class="rounded-full px-3 py-2 border border-red-500 flex items-center justify-center text-white hover:bg-red-500 transition-colors">Characters</router-link></li>
        <li> <router-link :to="'/Locations' " class="rounded-full px-3 py-2 border border-red-500 flex items-center justify-center text-white hover:bg-red-500 transition-colors">Locations</router-link></li>
      </ul>
      <div v-if="isMenuOpen" class="absolute top-12 right-4 bg-white text-black rounded-lg shadow-lg p-4">
        <p class="text-lg font-bold">Rebika Yihenew</p>
        <p class="text-sm mt-2">GitHub: <a href="https://github.com/Soloparame/Project.git" class="text-blue-500 hover:underline">View Github</a></p>
        <p class="text-sm mt-2">Figma: <a href="https://www.figma.com/design/zRYAkx5yLpDFflh3jrO7CS/Untitled?node-id=0-1&t=t7iNwkwfRH7wuepM-0" class="text-blue-500 hover:underline">View Design</a></p>
      </div>
    </nav>

    <main class="p-8 flex-grow flex flex-col items-center ">
      <div v-if="loading" class="text-center">Loading...</div>
      <div v-if="error" class="text-center text-red-500">Error loading location details</div>
      <div v-if="result" class="max-w-2xl mx-auto bg-gray-800 p-6 rounded-lg shadow-lg border-2 border-red-500 ">
        <h1 class="text-4xl font-bold text-center mt-4">{{ result.location.name }}</h1>
        <p class="text-center mt-2"><strong>Type:</strong> {{ result.location.type }}</p>
        <p class="text-center mt-2"><strong>Dimension:</strong> {{ result.location.dimension }}</p>
        <h3 class="text-2xl font-bold mt-6 mb-4">Residents</h3>
        <ul class="grid grid-cols-2 gap-4">
          <li v-for="resident in result.location.residents" :key="resident.id" class="flex items-center space-x-4">
            <router-link :to="'/characters/' + resident.id" class="inline-block relative"><img :src="resident.image" alt="" class="w-20 h-20 rounded-full border-2 border-red-500 hover:scale-110" /></router-link>
            <router-link :to="'/characters/' + resident.id" class="inline-block relative"><span>{{ resident.name }}</span></router-link>
          </li>
        </ul>
      </div>
    </main>

    <footer class="bg-[#1e1e1e] p-16 text-center border-t border-gray-700">
  <div class="text-white">
    <p class="text-2xl font-bold mb-6">Rebika Yihenew</p>
    <div class="flex flex-col md:flex-row justify-center items-center space-y-4 md:space-x-8 md:space-y-0">
      <p class="text-gray-400 text-lg flex items-center">
        <svg class="w-6 h-6 text-red-500 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path>
        </svg>
        GitHub account: <a href="https://github.com/Soloparame/Project.git" class="text-red-500 hover:underline ml-1">View Github</a>
      </p>
      <p class="text-gray-400 text-lg flex items-center">
        <svg class="w-6 h-6 text-red-500 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6.85 17.6a2.7 2.7 0 01-.7-.5L3 14.95l1.35-1.35a6 6 0 015.3-1.55M12 12v.01M12 3l9.6 9.6a2 2 0 010 2.83L12 24l-1.35-1.35"></path>
        </svg>
        Figma Design: <a href="https://www.figma.com/design/zRYAkx5yLpDFflh3jrO7CS/Untitled?node-id=0-1&t=t7iNwkwfRH7wuepM-0" class="text-red-500 hover:underline ml-1">View Design</a>
      </p>
    </div>
  </div>
</footer>
  </div>
</template>



