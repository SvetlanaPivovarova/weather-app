<template>
  <div class="background">
    <Header />
    <Teleport to="body">
      <modal :show="showModal" @close="showModal = false" >
        <template #body>
          <h1>Hello</h1>
        </template>
      </modal>
    </Teleport>
    <RouterView />
  </div>

</template>

<script>
import Header from "./components/Header.vue";
import Modal from "./components/Modal.vue";
export default {
  components: {
    Header,
    Modal
  },
  data() {
    return {
      showModal: false,
      isLoginSuccessful: false
    }
  },
  watch: {
    'showModal'() {
      this.isLoginSuccessful = false;
    },
    'isLoginSuccessful'() {
      if (this.isLoginSuccessful) {
        this.showModal = false;
      }
    }
  },
  mounted() {
    window.addEventListener('openModel', this.openModalWindow)
  },
  beforeUnmount() {
    window.removeEventListener('openModel', this.openModalWindow)
  },
  methods: {
    openModalWindow() {
      this.showModal = true;
    },
  }
}
</script>

