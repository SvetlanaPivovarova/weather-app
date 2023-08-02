<script setup>
import { ref, defineProps } from 'vue';
import Modal from "./Modal.vue";
defineProps({
  msg: {
    type: String,
    required: false
  }
})
const showMessage = ref(false);
const show = ref(false);
const toggle = () => {
  showMessage.value = !showMessage.value
  console.log(showMessage.value)
};
const showModal = () => {
  show.value = !show.value;
  console.log(show.value);
}
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>

    <button @click="toggle">Toggle</button>

    <Teleport to="#modal-container">
      <button @click="showModal">Войти</button>
      <modal :show="show" @close="show = false">
        <template #body>
          <h1>Hi, Bublik!</h1>
          <p>here we go</p>
        </template>
      </modal>
    </Teleport>

    <Teleport to="#purple-box">
      <p v-if="showMessage">Hello from HelloWorld {{showMessage}}</p>
    </Teleport>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
