<template>
  <section class="b-settings">
    <h2 class="b-settings--title">Settings</h2>
    <ul v-for="item in locations" :key="item">
      <LocationItem :locationDisplay="item" />
    </ul>
    <AddLocation @addLocation="locations = [...$event]" />
  </section>
  <button id="find-me" @click="geoFindMe">Show my location</button><br />
  <p id="status"></p>
  <a id="map-link" target="_blank"></a>

</template>

<script>
import LocationItem from "./LocationItem.vue";
import AddLocation from "./AddLocationForm.vue";

export default {
  name: 'Settings',
  components: { LocationItem, AddLocation },
  data() {
    return {
      locations: [],
      longitude: null,
      latitude: null,
      error: '',
      message: ''
    }
  },

}
</script>