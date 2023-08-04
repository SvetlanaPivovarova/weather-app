<script setup>
import { ref } from 'vue'
import Header from "./components/Header.vue";
import WeatherItem from "./components/WeatherItem.vue";

const locationName = ref('');

</script>

<template>
  <div class="background">
    <Header :location="locationName" />
    <WeatherItem @send-name="locationName = $event"/>
  </div>
</template>


