<template>
  <main>
    <div v-for="item in arrayOfLocations" :key="item.id">
      <Block :location-name="item.name"/>
    </div>
  </main>
</template>

<script>
import Block from "@/components/Block.vue";
import {locationStorage} from "@/utils/utils";
export default {
  components: {
    Block
  },
  data() {
    return {
      arrayOfLocations: locationStorage.fetch()
    }
  }
}
</script>
