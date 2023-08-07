<template>
  <div class="background">
    <HeaderComponent :location="locationName" />
    <WeatherItem @send-name="locationName = $event" />
  </div>
</template>

<script>
import HeaderComponent from './Header.vue';
import WeatherItem from "@/components/WeatherItem.vue";
export default {
  name: 'BlockComponent',
  components: {HeaderComponent, WeatherItem},
  data() {
    return {
      locationName: ''
    }
  }
}
</script>