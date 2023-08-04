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
  name: 'SettingsComponent',
  components: { LocationItem, AddLocation },
  data() {
    return {
      locations: []
    }
  },
  methods: {
    geoFindMe() {
  const status = document.querySelector("#status");
  const mapLink = document.querySelector("#map-link");

  mapLink.href = "";
  mapLink.textContent = "";

  function success(position) {
    const latitude = position.coords.latitude;
    const longitude = position.coords.longitude;

    status.textContent = "";
    mapLink.href = `https://www.openstreetmap.org/#map=18/${latitude}/${longitude}`;
    mapLink.textContent = `Широта: ${latitude} °, Долгота: ${longitude} °`;
  }

  function error() {
    status.textContent = "Невозможно получить ваше местоположение";
  }

  if (!navigator.geolocation) {
    status.textContent = "Geolocation не поддерживается вашим браузером";
  } else {
    status.textContent = "Определение местоположения…";
    navigator.geolocation.getCurrentPosition(success, error);
  }
}

}
}
</script>