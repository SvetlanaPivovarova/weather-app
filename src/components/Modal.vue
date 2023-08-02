<template>
  <div v-if="show" class="modal-mask">
    <div class="modal-container">
      <div class="modal-body">
        <slot name="body">default body</slot>
      </div>
      <button
          class="close-button icon icon-26 icon-close-button"
          @click="show = false"
      >Close</button>
    </div>
  </div>
</template>

<script>


export default {
  name: 'Modal',
  props: {
    show: Boolean
  },

}
</script>

<style>
.modal {
  position: absolute;
  top: 0; right: 0; bottom: 0; left: 0;
  background-color: rgba(0,0,0,.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.modal div {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: white;
  width: 300px;
  height: 100%;
  padding: 5px;
}

.modal-mask {
  position: fixed;
  z-index: 100;
  left: 0;
  top: 0;
  width: 100%;
  background-color: rgba(255, 255, 255, 0.75);
  display: flex;
  transition: opacity 0.3s ease;
  backdrop-filter: blur(7.5px);
}

.modal-container {
  width: 500px;
  margin: 50px auto 0;
  padding: 15px 30px 30px;
  background-color: #fff;
  border-radius: 16px;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.30);
  transition: all 0.3s ease;
  position: relative;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

</style>