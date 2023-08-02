<template>
  <header class="b-header">
    <div class="b-header--wrapper">
      <div class="icon icon-44 icon-location"></div>
      <h2 class="b-header--heading">{{ location }}</h2>
    </div>
    <div>
      <div class="icon icon-32 icon-settings"></div>
    </div>
  </header>
</template>

<script>
export default {
  props: {
    location: {
      type: String,
      default: 'London, UK'
    }
  }
}
</script>