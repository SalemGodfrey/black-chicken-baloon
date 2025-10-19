<template>
  <button
    class="start-button"
    @mousedown="startHold"
    @touchstart="startHold"
    @mouseup="endHold"
    @mouseleave="endHold"
    @touchend="endHold"
  >
    <svg viewBox="0 0 24 24" fill="none" class="fire" xmlns="http://www.w3.org/2000/svg">
      <g id="SVGRepo_iconCarrier">
        <path
          d="M5.926 20.574a7.26 7.26 0 0 0 3.039 1.511c.107.035.179-.105.107-.175-2.395-2.285-1.079-4.758-.107-5.873.693-.796 1.68-2.107 1.608-3.865 0-.176.18-.317.322-.211 1.359.703 2.288 2.25 2.538 3.515.394-.386.537-.984.537-1.511 0-.176.214-.317.393-.176 1.287 1.16 3.503 5.097-.072 8.19-.071.071 0 .212.072.177a8.761 8.761 0 0 0 3.003-1.442c5.827-4.5 2.037-12.48-.43-15.116-.321-.317-.893-.106-.893.351-.036.95-.322 2.004-1.072 2.707-.572-2.39-2.478-5.105-5.195-6.441-.357-.176-.786.105-.75.492.07 3.27-2.063 5.352-3.922 8.059-1.645 2.425-2.717 6.89.822 9.808z"
          fill="#db2c58"
        ></path>
      </g>
    </svg>
    <span class="start-button-title">start</span>
    <span class="start-button-subtitle">PRESS AND<br />HOLD</span>
  </button>
</template>

<script setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['button-touch', 'hold-start', 'hold-end'])

let holdInterval = null

function startHold() {
  if (!holdInterval) {
    emit('button-touch')
    holdInterval = setInterval(() => {
      emit('hold-start')
    }, 1000)
  }
}

function endHold() {
  if (holdInterval) {
    clearInterval(holdInterval)
    holdInterval = null
    emit('hold-end')
  }
}
</script>

<style>
.start-button {
  padding: 10px 15px 10px 15px;
  text-transform: uppercase;
  border-radius: 100%;
  outline: none;
  border: none;
  background: #6a214a;
  background: linear-gradient(180deg, rgba(106, 33, 74, 1) 0%, rgba(224, 38, 79, 1) 100%);
  color: white;
  display: flex;
  align-items: center;
  flex-direction: column;
  box-shadow:
    inset 0 5px 5px rgba(255, 255, 255, 0.3),
    inset 0 -5px 5px rgba(0, 0, 0, 0.369);
  cursor: pointer;
  border: 3px solid #d6d6d6;
  outline: 2px solid #ffffff;
}
@keyframes pulse {
  0% {
    box-shadow:
      0 0 0 0 rgba(219, 44, 88, 0.7),
      inset 0 5px 5px rgba(255, 255, 255, 0.3),
      inset 0 -5px 5px rgba(0, 0, 0, 0.369);
    transform: scale(1);
  }
  70% {
    box-shadow: 0 0 0 15px rgba(219, 44, 88, 0);
    transform: scale(1.08);
  }
  100% {
    box-shadow:
      0 0 0 0 rgba(219, 44, 88, 0),
      inset 0 5px 5px rgba(255, 255, 255, 0.3),
      inset 0 -5px 5px rgba(0, 0, 0, 0.369);
    transform: scale(1);
  }
}

.start-button:active,
.start-button.hold {
  animation: none !important;
  box-shadow: inset 0 5px 5px rgba(0, 0, 0, 0.369);
  transform: scale(1.03);
}

.start-button {
  animation: pulse 1.4s infinite;
  transition: transform 0.2s;
}

.start-button:hover {
  box-shadow: inset 0 5px 5px rgba(0, 0, 0, 0.369);
}

.start-button-title {
  font-size: 1rem;
  text-transform: uppercase;
  font-family: 'Dela Gothic One';
  text-shadow: 0 0 10px rgb(255, 244, 212);
}

.fire {
  width: 40px;
  height: 40px;
}

.start-button-subtitle {
  font-size: 0.6rem;
  text-transform: uppercase;
  font-family: 'Encode Sans Expanded';
}
</style>
