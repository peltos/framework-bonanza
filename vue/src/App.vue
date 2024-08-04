<template>
  <div class="container mx-auto py-4">
    <h1 class="text-4xl text-center">Pokedex VueJS</h1>
    <Search @getData="fetchData" />
    <div v-if="data" class="bg-gray-200 sm:w-[500px] mx-auto flex flex-col items-center gap-4 p-4 rounded-md">
      <div v-if="data && !loading" class="flex flex-col items-center gap-4 ">
        <div>
          <h2 class="text-xl capitalize">{{ data.name }} #{{ data.id }}</h2>
          <ul class="flex flex-wrap gap-2 justify-center text-lg">
            <li v-for="(type, index) in data.types" class=""> {{ type.type.name }}</li>
          </ul>
        </div>
        <div class="flex flex-wrap justify-center">
          <img v-if="data.sprites.front_default" :src="data.sprites.front_default" alt="pokemon front" class="w-32">
          <img v-if="data.sprites.back_default" :src="data.sprites.back_default" alt="pokemon back" class="w-32">
        </div>

        <h3 class="text-xl capitalize">Stats</h3>
        <ul class="grid gap-2 sm:w-[408px]">
          <li v-for="(stat, index) in data.stats" class="relative grid sm:grid-cols-2 gap-2">
            <p class="capitalize relative z-10"><strong>{{ stat.stat.name }}:</strong> {{ stat.base_stat }}</p>
            <div :class="'right-full top-0 h-6 bg-' + colors[index] + '-500 z-0'" :style="'width:' + stat.base_stat + 'px'">
            </div>
          </li>
        </ul>

        <h3 class="text-xl capitalize">abilities</h3>
        <ul>
          <li v-for="(ability) in data.abilities" class="relative grid gap-2">
            <p class="capitalize relative z-10">{{ ability.ability.name }}</p>
          </li>
        </ul>
      </div>
      <p v-if="loading && !error">
        Loading..
      </p>
      <p v-if="error">
        {{ error }}
      </p>
    </div>
  </div>
  <div class="hidden bg-red-500 bg-orange-500 bg-yellow-500 bg-green-500 bg-blue-500 bg-purple-500"></div>
</template>

<script setup>
import Search from './components/Search.vue'
import { ref, onMounted } from "vue";
const data = ref(null);
const loading = ref(true);
const error = ref(null);
const colors = ref(['red', 'orange', 'yellow', 'green', 'blue', 'purple']);

function fetchData(pokemon = 'lotad') {
  loading.value = true;
  // I prefer to use fetch
  // you can use use axios as an alternative
  return fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon}`, {
    method: 'get',
    headers: {
      'content-type': 'application/json'
    }
  })
    .then(res => {
      // a non-200 response code
      if (!res.ok) {
        // create error instance with HTTP status text
        const error = new Error(res.statusText);
        error.json = res.json();
        throw error;
      }

      return res.json();
    })
    .then(json => {
      // set the response data
      data.value = json;
      error.value = null;

    })
    .catch(err => {
      error.value = err;
      // In case a custom JSON error response was provided
      if (err.json) {
        return err.json.then(json => {
          // set the JSON response message
          error.value.message = json.message;
        });
      }
    })
    .then(() => {
      loading.value = false;
    });
}

onMounted(() => {
  fetchData();
});
</script>