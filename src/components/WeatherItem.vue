<template>
  <div v-if="!isLoading" class="b-info">
    <div class="b-info--section">
      <div class="icon icon-100 icon-weather"></div>
      <div class="b-info--wrapper">
        <p class="b-info--number">{{ temp }} <span>&#8451;</span></p>
      </div>
    </div>
    <div class="b-info--text">Feels like {{ feels }} ℃, {{ description }}</div>
  </div>
  <Info
      v-if="!isLoading"
      :wind="wind"
      :humidity="humidity"
      :pressure="pressure"
      :visibility="visibility"
  />
  <div v-if="isLoading" class="b-info--loader-container">
    <Loader />
  </div>
  <Modal :show="showModal" @close="showModal = false" >
    <template #body>
      <p class="b-info--tooltip">{{ this.error }}</p>
      <Settings />
    </template>
  </Modal>
</template>

<script>
import Info from "./Info.vue";
import Modal from "@/components/Modal.vue";
import { API_KEY } from "../utils/constants";
import Loader from "./Loader.vue";
import Settings from "@/components/Settings.vue";

export default {
  name: 'WeatherItem',
  components: {Settings, Loader, Info, Modal },
  data() {
    return {
      latitude: null,
      longitude: null,
      error: '',
      message: '',
      results: null,
      isLoading: null,
      temp: null,
      wind: null,
      humidity: null,
      pressure: null,
      visibility: null,
      feels: null,
      description: '',
      showModal: false
    }
  },
  emits: ['sendName', 'sendShow'],
  mounted() {
    this.isLoading = true;
    this.geoFindMe()
  },
  methods: {
    handleSuccess(position) {
      fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=${API_KEY}&units=metric`)
          .then((response) => response.json())
          .then((result) => {
            this.results = result;
          })
          .catch((error) => {
            console.log(error);
          })
          .finally(() => this.isLoading = false);
    },
    handleError() {
      this.error = "Unable to get your location. Select below";
      this.isLoading = false;
      this.showModal = true;

      alert(this.error)
    },
    geoFindMe() {

      if (!navigator.geolocation) {
        this.error = "Geolocation не поддерживается вашим браузером";

      } else {
        this.message = "Определение местоположения…";
        navigator.geolocation.getCurrentPosition(this.handleSuccess, this.handleError);
      }
    },
  },
  watch: {
    'results'() {
      this.temp = Math.round(this.results.main.temp);
      this.wind = Number(this.results.wind.speed);
      this.humidity = Number(this.results.main.humidity);
      this.pressure = Number(this.results.main.pressure);
      this.visibility = Number(this.results.visibility) / 1000;
      this.feels = Math.round(this.results.main.feels_like);
      this.description = this.results.weather[0].description;
      this.$emit('sendName', this.results.name)
    }
  }
}
</script>