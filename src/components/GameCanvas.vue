<template>
  <div ref="gameContainer" class="game-container">
    <ModalWin :isModalVisible="state.isModalVisible" @tryAgain="tryAgain" />
    <BalanceBox :balanceValue="state.balance" />
    <CockImage :isCockUp="state.isCockUp" :isCockDead="state.isCockDead" />
    <div class="bet-block">
      <StartButton @button-touch="onButtonTouch" @hold-start="onHoldStart" @hold-end="onHoldEnd" />
      <BetBox :betValue="state.bet" @update:betValue="state.bet = $event" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ModalWin from './HUD/ModalWin.vue'
import BalanceBox from './HUD/BalanceBox.vue'
import StartButton from './HUD/StartButton.vue'
import BetBox from './HUD/BetBox.vue'
import CockImage from './HUD/CockImage.vue'
import { reactive } from 'vue'

const gameContainer = ref(null)
const state = reactive({
  balance: 1000000,
  bet: 100,
  isHolding: false,
  secsHold: 0,
  needStart: true,
  holdTimer: null,
  killZone: 0,
  isCockUp: true,
  isCockDead: false,
  isModalVisible: false,
})

function onButtonTouch() {
  if (!state.needStart) return // защита
  console.log('clicked')
  state.isHolding = true
  state.needStart = false // запрещаем повторный старт удержания
  state.balance -= state.bet
  state.killZone = Math.floor(Math.random() * (14 - 3 + 1)) + 3
  console.log('killZone:', state.killZone)
  state.isCockUp = false
  state.isCockDead = false
}

function onHoldStart() {
  if (!state.isHolding || state.isCockDead) return // защита от удержания после проигрыша
  if (state.holdTimer) return // уже идёт таймер
  state.holdTimer = setInterval(() => {
    state.secsHold++
    console.log('secsHold:', state.secsHold)

    if (state.secsHold === state.killZone) {
      clearInterval(state.holdTimer)
      state.holdTimer = null
      console.log('killed')
      state.isCockDead = true
      state.isHolding = false
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
  if (!state.isHolding) return // если удержание не было, не делаем ничего
  if (state.holdTimer) {
    clearInterval(state.holdTimer)
    state.holdTimer = null
  }

  state.isHolding = false
  console.log('hold end')

  // проверяем, что игра ещё не окончена
  if (!state.isCockDead && !state.needStart) {
    state.balance += state.bet * 2
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

  // Перезагружаем видео чтобы оно началось с начала
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
