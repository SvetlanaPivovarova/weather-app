<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import Header from "./components/Header.vue";
const click = () => {
  alert("hello")
}
</script>

<template>
  <Header />

  <RouterView />
</template>

