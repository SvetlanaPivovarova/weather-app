<template>
  <header class="b-header">
    <div class="b-header--wrapper">
      <div class="icon icon-44 icon-location"></div>
      <h2 class="b-header--heading">{{ location }}</h2>
    </div>
    <div>
      <button class="b-header--button icon icon-settings"  @click="showModal = true">
      </button>
    </div>
  </header>
  <Modal :show="showModal" @close="showModal = false" >
    <template #body>
      <Settings />
    </template>
  </Modal>

</template>

<script>
import Modal from "./Modal.vue";
import Settings from "./Settings.vue";
export default {
  props: {
    location: {
      type: String,
      default: 'London, UK'
    }
  },
  data() {
    return {
      showModal: false,
      loc: null
    }
  },
  components: {
    Settings,
    Modal
  },
  mounted() {
    //this.loc = window.getCurrentPosition();
    if ("geolocation" in navigator) {
      navigator.geolocation.getCurrentPosition(function(position) {
        alert(position.coords.latitude, position.coords.longitude);
      });
    } else {
      console.log('no')
    }

  },
  methods: {
    changeSettings() {
      alert('click')
    }
  }
}
</script>