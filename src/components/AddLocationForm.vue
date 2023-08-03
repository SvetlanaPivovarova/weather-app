<template>
  <div class="form-control">
    <label class="input-text-label" for="text">Add location</label>
    <div class="form-control--container">
      <input
          class="input-text"
          type="text"
          id="text"
          placeholder="Your new location to observe the weather"
          v-model="newLocation"
      />
      <button
          class="icon icon-58 icon--add button--add"
          :disabled="isValid"
          @click="addLocation"
      >
      </button>
    </div>

  </div>


</template>

<script>
export default {
  name: 'AddLocation',
  data() {
    return {
      newLocation: null,
      locations: {
        type: Array
      },
      isValid: false,
    }
  },
  methods: {
    addLocation() {
      this.locations = this.locations.push(...this.newLocation)
      console.log(this.locations)
      this.$emit('addLocation', this.newLocation)
    }
  }
}
</script>