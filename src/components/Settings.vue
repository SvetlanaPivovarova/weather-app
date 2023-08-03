<template>
  <section class="b-settings">
    <h2 class="b-settings--title">Settings</h2>
    <ul v-for="item in locations" :key="item">
      <LocationItem location-display="item" />
    </ul>
    <AddLocation @addLocation="locations = [...$event]" />
  </section>
</template>

<script>
import LocationItem from "./LocationItem.vue";
import AddLocation from "./AddLocationForm.vue";

export default {
  name: 'Settings',
  components: { LocationItem, AddLocation },
  data() {
    return {
      locations: []
    }
  }
}
</script>