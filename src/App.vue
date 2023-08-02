<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Header from "./components/Header.vue";
const click = () => {
  alert("hello")
}
</script>

<template>
  <div class="background">
    <Header />

    <RouterView />
  </div>

</template>

