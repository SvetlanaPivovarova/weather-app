<template>
  <header class="b-header">
    <div class="b-header--wrapper">
      <div class="icon icon-44 icon-location"></div>
      <h2 class="b-header--heading">{{ location }}</h2>
    </div>
    <div>
      <button class="b-header--button icon icon-32 icon-settings"  @click="showModal = true">
      </button>
    </div>
  </header>
  <Teleport to="#modal-container">
    <Modal :show="showModal" @close="showModal = false" >
      <template #body>
        <h1>Hi, Bublik!</h1>
        <p>here we go</p>
      </template>
    </Modal>
  </Teleport>

</template>

<script>
import Modal from "./Modal.vue";
export default {
  props: {
    location: {
      type: String,
      default: 'London, UK'
    }
  },
  data() {
    return {
      showModal: false
    }
  },
  components: {
    Modal
  },
  methods: {
    changeSettings() {
      alert('click')
    }
  }
}
</script>