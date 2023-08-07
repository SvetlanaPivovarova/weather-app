<template>
  <div v-if="!isLoading" class="b-info">
    <div class="b-info--section">
      <div class="icon icon-100 icon-weather"></div>
      <div class="b-info--wrapper">
        <p class="b-info--number">{{ weatherData.temp }} <span>&#8451;</span></p>
      </div>
    </div>
    <div class="b-info--text">Feels like {{ weatherData.feels }} ℃, {{ weatherData.description }}</div>
  </div>
  <Info
      v-if="!isLoading"
      :wind="weatherData.wind"
      :humidity="weatherData.humidity"
      :pressure="weatherData.pressure"
      :visibility="weatherData.visibility"
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
import {API_KEY} from "../utils/constants";
import Loader from "./Loader.vue";
import Settings from "@/components/Settings.vue";

import { locationStorage } from "@/utils/utils";

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
      showModal: false,
      locations: locationStorage.fetch(),
      weatherData: {
        name: null,
        temp: null,
        wind: null,
        humidity: null,
        pressure: null,
        visibility: null,
        feels: null,
        description: '',
      },
      allTheWeather: []
    }
  },
  emits: ['sendName', 'sendShow'],
  mounted() {
    this.isLoading = true;
    if(!locationStorage.fetch()) {
      this.geoFindMe()
    }
    else {
      for (let i = 0; i < this.locations.length; i++) {
        fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.locations[i].name}&appid=${API_KEY}&units=metric`)
            .then((response) => response.json())
            .then((result) => {
              //this.results = result;
              //this.locations.push({
              //  id: locationStorage.uid++,
              //  name: result.name,
              //});
              //locationStorage.save(this.locations);
              console.log('done', i, result)
              localStorage.setItem(`${this.locations[i].name}`, JSON.stringify(result));

            })
            .catch((error) => {
              console.log(error);
            })
            .finally(() => this.isLoading = false);
      }

    }

  },
  methods: {
    handleSuccess(position) {
      fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=${API_KEY}&units=metric`)
          .then((response) => response.json())
          .then((result) => {
            this.results = result;
            this.locations.push({
              id: locationStorage.uid++,
              name: result.name,
            });
            locationStorage.save(this.locations);
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
      this.weatherData.temp = Math.round(this.results.main.temp);
      this.weatherData.wind = Number(this.results.wind.speed);
      this.weatherData.humidity = Number(this.results.main.humidity);
      this.weatherData.pressure = Number(this.results.main.pressure);
      this.weatherData.visibility = Number(this.results.visibility) / 1000;
      this.weatherData.feels = Math.round(this.results.main.feels_like);
      this.weatherData.description = this.results.weather[0].description;
      this.weatherData.name = this.results.name;
      localStorage.setItem(`${this.results.name}`, JSON.stringify(this.weatherData));
      this.$emit('sendName', this.results.name)
    }
  }
}
</script>