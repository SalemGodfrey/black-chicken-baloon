<template>
  <div ref="gameContainer" class="game-container">
    <div class="timer">
      <span>$</span>
      <span>{{ state.bet }}</span
      ><span class="cents">.{{ formattedCents }}</span>
    </div>

    <ModalWin :isModalVisible="state.isModalVisible" @tryAgain="tryAgain" />
    <BalanceBox :balanceValue="formattedBalance" />
    <CockImage :isCockUp="state.isCockUp" :isCockDead="state.isCockDead" />

    <div class="bet-block">
      <StartButton @button-touch="onButtonTouch" @hold-start="onHoldStart" @hold-end="onHoldEnd" />
      <BetBox :betValue="state.bet" @update:betValue="state.bet = $event" />
    </div>
  </div>
</template>

<script setup>
import { Howl, Howler } from 'howler'
import { ref, reactive, computed } from 'vue'
import ModalWin from './HUD/ModalWin.vue'
import BalanceBox from './HUD/BalanceBox.vue'
import StartButton from './HUD/StartButton.vue'
import BetBox from './HUD/BetBox.vue'
import CockImage from './HUD/CockImage.vue'

const gameContainer = ref(null)
// Howler.js may not play sounds unless triggered by a user interaction due to browser autoplay policy.
import themeMp3 from '@/assets/theme.mp3'

// Play sound on page load
window.addEventListener('DOMContentLoaded', () => {
  var sound = new Howl({
    src: [themeMp3],
    autoplay: true,
    loop: true,
    volume: 0.5,
  });
  sound.play();
});

const state = reactive({
  balance: 1000000, // центы, чтобы не терять точность
  bet: 100, // доллары
  cents: 0, // центы от 0 до 99
  isHolding: false,
  secsHold: 0,
  needStart: true,
  holdTimer: null,
  killZone: 0,
  isCockUp: true,
  isCockDead: false,
  isModalVisible: false,
  msTimer: null,
})

// 👇 Форматированный баланс в долларах
const formattedBalance = computed(() => {
  const dollars = Math.floor(state.balance / 100)
  const cents = state.balance % 100
  return `$${dollars}.${cents.toString().padStart(2, '0')}`
})

// 👇 Форматированные центы для таймера
const formattedCents = computed(() => state.cents.toString().padStart(2, '0'))

function onButtonTouch() {
  if (!state.needStart) return
  console.log('clicked')
  state.isHolding = true
  state.needStart = false

  if (state.msTimer) {
    clearInterval(state.msTimer)
    state.msTimer = null
  }

  state.cents = 0
  state.msTimer = setInterval(() => {
    state.cents += 1
    if (state.cents >= 100) {
      state.cents = 0
      state.bet += 1
    }
  }, 100)

  // баланс в центах
  state.balance -= state.bet * 100
  state.killZone = Math.floor(Math.random() * (14 - 3 + 1)) + 3
  console.log('killZone:', state.killZone)
  state.isCockUp = false
  state.isCockDead = false
}

function onHoldStart() {
  if (!state.isHolding || state.isCockDead) return
  if (state.holdTimer) return

  state.holdTimer = setInterval(() => {
    state.secsHold++
    console.log('secsHold:', state.secsHold)

    if (state.secsHold === state.killZone) {
      clearInterval(state.holdTimer)
      state.holdTimer = null
      console.log('killed')
      state.isCockDead = true
      state.isHolding = false
      if (state.msTimer) {
        clearInterval(state.msTimer)
        state.msTimer = null
      }
      setTimeout(() => tryAgain(), 3000)

      let cockUp = document.querySelector('.dead-cock')
      if (cockUp) {
        cockUp.style.transition = 'transform 1s cubic-bezier(0.65, 0, 0.35, 1)'
        cockUp.style.transform = 'translate(-50%, 200%)'
      }
    }

    let container = document.querySelector('.game-container')
    if (container) {
      container.style.transition = 'background-position-y 10s ease-in'
      container.style.backgroundPositionY = '0%'
    }
  }, 1000)
}

function onHoldEnd() {
  if (!state.isHolding) return

  if (state.holdTimer) {
    clearInterval(state.holdTimer)
    state.holdTimer = null
  }

  if (state.msTimer) {
    clearInterval(state.msTimer)
    state.msTimer = null

    // вычислим выигрыш: 10% от ставки + доля центов
    const betCents = state.bet * 100 + state.cents
    const reward = Math.floor(betCents * 0.1)
    state.balance += reward

    // сброс ставки
    state.bet = 100
    state.cents = 0
  }

  state.isHolding = false
  console.log('hold end')

  if (!state.isCockDead && !state.needStart) {
    // выигрыш: удвоение ставки
    state.balance += state.bet * 200
    console.log('win')
    state.isModalVisible = true
  }

  state.secsHold = 0
}

function tryAgain() {
  console.log('try again')
  state.isModalVisible = false
  state.isCockUp = true
  state.isCockDead = false
  state.isHolding = false
  state.secsHold = 0
  state.needStart = true

  if (state.msTimer) {
    clearInterval(state.msTimer)
    state.msTimer = null
  }
  state.cents = 0

  let cockUp = document.querySelector('.dead-cock')
  if (cockUp) {
    cockUp.currentTime = 0
    cockUp.load()
  }

  let cockIdle = document.querySelector('.idle')
  if (cockIdle) {
    cockIdle.style.display = 'block'
  }
  if (cockUp) {
    cockUp.style.display = 'none'
    cockUp.style.transform = 'translate(-50%, 0%)'
  }

  let container = document.querySelector('.game-container')
  if (container) {
    container.style.transition = 'background-position-y 0s linear'
    container.style.backgroundPositionY = '100%'
  }
}
</script>

<style>
.timer {
  color: white;
  font-size: 3rem;
  font-family: 'Dela Gothic One';
  position: absolute;
  top: 10%;
  z-index: 1000;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.sound-button {
  position: absolute;
  top: 7%;
  right: 1%;
  z-index: 2000;
  background-color: transparent;
  border: none;
  outline: none;
}

.game-container {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden;
  position: relative; /* обязательно для absolute элементов HUD */
  background-image: url('@/assets/background.png');
  background-position-y: 100%;
  background-position-x: center;
}

/* HUD */

.bet-block {
  height: 100px;
  padding: 0 20px;
  background-color: #1c1c21;
  border-radius: 10px;
  color: #c8c8c9;
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  z-index: 2;
  width: 100%;
  max-width: 39rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* BalanceBox примерно */
BalanceBox {
  position: absolute;
  top: 2rem;
  left: 2rem;
  z-index: 10;
}
</style>
