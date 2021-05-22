<template>
  <div id="app" class="app" :class="{'modal-open': isModalVisible}">
    <button type="button" class="btn-open" @click="showModal">
      Налоговый вычет
    </button>

    <Modal v-show="isModalVisible" @close="closeModal" />
  </div>
</template>
<script>
import Modal from "./components/Modal.vue";

export default {
  name: "App",
  components: {
    Modal,
  },
  data() {
    return {
      isModalVisible: false,
    };
  },
  methods: {
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  font-family: 'Lab Grotesque';
  box-sizing: border-box;
}

.app {
  width: 100%;
  height: 100vh;
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-open{
  background: rgba(0, 0, 0, 0.3);;
}

.btn-open {
  width: 198px;
  height: 56px;
  border: 1px solid #ffffff;
  box-sizing: border-box;
  filter: drop-shadow(0px 0px 44px #cfdae7);
  border-radius: 6px;
  background: transparent;
  color: #fff;
  font-family: Lab Grotesque;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: #ffffff;
  cursor: pointer;
}

button:hover {
  background: #fff;
  color: #000;
}

button:disabled {
  background: #bec5cc;
}
</style>
