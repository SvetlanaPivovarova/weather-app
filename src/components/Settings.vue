<template>
  <section class="b-settings">
    <h2 class="b-settings--title">Settings</h2>
    <ul v-for="item in locations" :key="item">
      <LocationItem :locationDisplay="item" @delete="deletedItem = $event" />
    </ul>
    <AddLocationForm @add="newItem = $event" />
  </section>
</template>

<script>
import LocationItem from "./LocationItem.vue";
import AddLocationForm from "./AddLocationForm.vue";

// localStorage persistence
const STORAGE_KEY = "weather-locations";
const locationStorage = {
  fetch() {
    const locations = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    locations.forEach((location, index) => {
      location.id = index;
    });
    locationStorage.uid = locations.length;
    return locations;
  },
  save(locations) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(locations));
  }
};

export default {
  name: 'SettingsComponent',
  components: { AddLocationForm, LocationItem },
  data() {
    return {
      locations: locationStorage.fetch(),
      newItem: null,
      deletedItem: ''
    }
  },
  watch: {
    'locations'() {
      locationStorage.save(this.locations)
    },
    'newItem'() {
      this.addLocation(this.newItem)

    },
    'deletedItem'() {
      this.removeLocation(this.deletedItem)
    }
  },
  methods: {
    addLocation() {
      const value = this.newItem && this.newItem.trim();
      if (!value) {
        return;
      }
      this.locations.push({
        id: locationStorage.uid++,
        name: value,
      });
      locationStorage.save(this.locations);
      this.newItem = "";
    },
    removeLocation(id) {
      this.locations = this.locations.filter(
          (item) => item.id !== id
      );
      locationStorage.save(this.locations);
    },
  }
}
</script>