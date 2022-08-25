<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { ref } from "vue";
import Hero from "./components/Hero.vue";
import StandardLayout from './layouts/StandardLayout.vue'
import Home from "./components/Home.vue";

import 'https://flackr.github.io/scroll-timeline/dist/scroll-timeline.js';

let pageViewIdx = ref(0);
</script>

<template>
  <Hero v-if="pageViewIdx === 0" @changePageIdx="(v) => (pageViewIdx = v)" />
  <StandardLayout v-else-if="pageViewIdx > 0" @changePageIdx="(v) => (pageViewIdx = v)">
    <Home v-if="pageViewIdx === 1" />
  </StandardLayout>
</template>

<style scoped>
</style>
