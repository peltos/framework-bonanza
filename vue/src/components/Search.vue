<template>
  <div class="p-4 flex flex-wrap justify-center gap-2">
    <input v-on:keyup.enter="$emit('getData', pokemon)" v-model="pokemon" class="bg-gray-100 border border-gray-600 px-3 py-2 rounded-md" type="text" placeholder="Enter pokemon...">
    <button @click="$emit('getData', pokemon)" class="bg-red-500 px-3 py-2 rounded-md font-bold">Submit</button>
  </div>
</template>

<script setup>
import { defineEmits, ref } from 'vue';
let pokemon = ref('lotad');
defineEmits(['getData'])
</script>