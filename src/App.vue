<script setup>
import Block from "@/components/Block.vue";
import { locationStorage } from "@/utils/utils";

const arrayOfLocations = locationStorage.fetch();
</script>

<template>
  <div v-for="item in arrayOfLocations" :key="item.id">
    <Block :location-name="item.name"/>
  </div>

</template>


