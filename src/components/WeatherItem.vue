<template>
  <div class="b-info">
    <div class="b-info--section">
      <div class="icon icon-100 icon-weather"></div>
      <div class="b-info--wrapper">
        <p class="b-info--number">{{ degree }} <span>&#8451;</span></p>
      </div>
    </div>
    <div class="b-info--text">{{ weatherDescription }}</div>
  </div>
  <Info />
</template>

<script>
import Info from "./Info.vue";

export default {
  name: 'WeatherItem',
  components: { Info },
  props: {
    degree: {
      type: Number,
      default: 7
    },
    weatherDescription: {
      type: String,
      default: 'Feels like -3 ℃. Broken clouds. Light Breeze'
    }
  }
}
</script>