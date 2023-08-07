<template>
  <div v-if="!isLoading" class="b-info">
    <div class="b-info--section">
      <div class="icon icon-100 icon-weather"></div>
      <div class="b-info--wrapper">
        <p class="b-info--number">{{ getTemperature }} <span>&#8451;</span></p>
      </div>
    </div>
    <div class="b-info--text">Feels like {{ getFeelsLike }} ℃, {{ getDescription }}</div>
  </div>
  <Info
      v-if="!isLoading"
      :wind="getWind"
      :humidity="getHumidity"
      :pressure="getPressure"
      :visibility="getVisibility"
  />
  <div v-if="isLoading" class="b-info--loader-container">
    <Loader />
  </div>
  <Modal :show="showModal" @close="showModal = false" >
    <template #body>
      <p class="b-info--tooltip">{{ error }}</p>
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
  props: {
    weather: {
      type: Object,
    }
  },
  data() {
    return {
      latitude: null,
      longitude: null,
      error: '',
      isLoading: null,
      showModal: false,
      locations: locationStorage.fetch(),
    }
  },
  computed: {
    getTemperature() {
      return Math.round(this.weather.main.temp)
    },
    getWind() {
      return Number(this.weather.wind.speed)
    },
    getHumidity() {
      return Number(this.weather.main.humidity)
    },
    getPressure() {
      return Number(this.weather.main.pressure)
    },
    getVisibility() {
      return Number(this.weather.visibility)
    },
    getFeelsLike() {
      return Math.round(this.weather.main.feels_like)
    },
    getDescription() {
      return this.weather.weather[0].description
    }
  },
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
        navigator.geolocation.getCurrentPosition(this.handleSuccess, this.handleError);
      }
    },
  },
}
</script>