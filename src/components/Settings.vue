<template>
  <section class="b-settings">
    <h2 class="b-settings--title">Settings</h2>
    <ul>
      <LocationItem />
    </ul>
  </section>
</template>

<script>
import LocationItem from "./LocationItem.vue";

export default {
  name: 'Settings',
  components: { LocationItem },

}
</script>