<template>
  <li class="b-settings--item">
    <div class="b-settings--line">
      <div class="b-settings--accent"><span></span></div>
      <p class="b-settings--item-text">{{ locationDisplay }}</p>
    </div>
    <div class="icon icon-24 icon-trash-delete"></div>
  </li>
</template>

<script>
export default {
  name: 'LocationItem',
  props: {
    locationDisplay: {
      type: String,
      default: 'London, UK'
    }
  }
}
</script>