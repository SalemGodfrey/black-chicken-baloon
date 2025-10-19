<template>
  <video autoplay playsinline muted v-if="props.isModalVisible" class="confeti">
    <source src="@/assets/confeti.webm" type="video/webm" />
  </video>
  <div class="modal-container" v-if="props.isModalVisible">
    <div class="modal">
      <p class="modal-title">YOU WON!</p>
      <div class="buttons">
        <button class="button1">CLAIM REWARD</button>
        <button @click="tryAgain" class="button2">PLAY AGAIN</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['try-again'])

const props = defineProps({
  isModalVisible: {
    type: Boolean,
    default: false,
  },
})
 function tryAgain() {
  emit('try-again')
 }
</script>

<style>
.confeti {
  position: absolute;
  top: 10rem;
  left: 50%;
  transform: translate(-50%);
  z-index: 1001;
  width: 30rem;
  max-width: 39rem;
  pointer-events: none;
}

.modal-container {
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.451);
  z-index: 1000;
  position: fixed;
  left: 0;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0;
}
.modal {
  background: #e3e3de;
  border: 2px solid white;
  padding: 20px 20px;
  border-radius: 10px;
  width: 100%;
  max-width: 20rem;
}
.modal-title {
  font-size: 34px;
  font-weight: bold;
  color: black;
  padding: 0;
  margin: 0 0;
  text-transform: uppercase;
  text-align: center;
  font-family: 'Dela Gothic One';
  letter-spacing: 1px;
}
.buttons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
  padding-top: 20px;
  z-index: 1004;
  position: relative;
}
.button1 {
  background: #ff1b2a;
  background: linear-gradient(61deg, rgba(255, 27, 42, 1) 0%, rgba(243, 239, 133, 1) 100%);
  outline: none;
  color: white;
  font-size: 1.2rem;
  padding: 10px 20px;
  font-family: 'Encode Sans Expanded';
  font-weight: 900;
  border-radius: 10px;
  border: none;
  box-shadow: 0 5px 10px #ff8412;
  transition: ease-in-out 0.2s;
  cursor: pointer;
  z-index: 1003;
  position: relative;
}
.button1:hover {
  box-shadow: 0 0 3px #ff8412;
  transform: translateY(2px);
}

.button2 {
  position: relative;
  outline: none;
  color: black;
  font-size: 1.2rem;
  padding: 10px 20px;
  font-family: 'Encode Sans Expanded';
  font-weight: 900;
  border: none;
  border-radius: 10px;
  box-shadow: 0 5px 10px #2173a0;
  transition: ease-in-out 0.2s;
  cursor: pointer;
  background:
    linear-gradient(#e3e3de, #e3e3de) padding-box,
    linear-gradient(90deg, #34ffff 0%, #2173a0 100%) border-box;
  border: 3px solid transparent;
  /* Ensures the radius applies to both background layers */
  background-origin: border-box;
  background-clip: padding-box, border-box;
  z-index: 1003;
  position: relative;
}
.button2:hover {
  box-shadow: 0 0 3px #2173a0;
  transform: translateY(2px);
}
</style>
