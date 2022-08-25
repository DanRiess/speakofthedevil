<script setup>
import { ref } from "vue";

const emit = defineEmits(['changePageIdx'])

const animations = ref({
  in: false,
  out: false,
})

function animateOut(e) {
  animations.value.out = true
  setTimeout(() => {
    animations.value.out = false
    emit('changePageIdx', 1)
  }, 750);
}
</script>

<template>
    <div class="logo-container" @wheel.passive="animateOut" @click="animateOut">
      <img :class="{'animate-out': animations.out}" src="../assets/hero_BG.png" />
      <img :class="{'animate-out': animations.out}" src="../assets/logo_hero_empty.png" />
    </div>
</template>

<style scoped>
.logo-container {
  position: relative;
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  perspective: 1000px;
  overflow: hidden;
}

.logo-container img {
  position: absolute;
  height: 100%;
  width: 100%;
  overflow: hidden;
  transition: transform 500ms ease, opacity 750ms ease;
}

.animate-out {
  transform: scale(2);
  opacity: 0;
}
</style>