<template>
  <div ref="gameContainer" class="game-container">
    <BalanceBox  :balance="balance" />
    <div class="bet-block">
      <StartButton @start="startGame"/>
      <BetBox />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import Phaser from 'phaser'
import BalanceBox from './HUD/BalanceBox.vue'
import StartButton from './HUD/StartButton.vue'
import BetBox from './HUD/BetBox.vue'
import bg from '@/assets/background.png'
import balloon from '@/assets/cock.gif'

const gameContainer = ref(null)

onMounted(() => {
  const config = {
    type: Phaser.AUTO,
    scale: {
      parent: gameContainer.value,
      mode: Phaser.Scale.RESIZE,
      autoCenter: Phaser.Scale.CENTER_BOTH,
    },
    scene: { preload, create, update },
  }

  const game = new Phaser.Game(config)

  window.addEventListener('resize', () => {
    game.scale.resize(gameContainer.value.clientWidth, gameContainer.value.clientHeight)
  })
})

function preload() {
  // тут спрайты, аудио и др.
  this.load.image('background', bg)
  this.load.image('balloon', balloon)
}

function create() {
  // тут создаёшь игровые объекты
  // Создаем фон переменную с фоном и устанавливаем позицию
  const bgImage = this.add.image(0, 0, 'background').setOrigin(0.2, 0)
  // Устанавливаем ширину и высоту фона
  bgImage.displayWidth = this.scale.width * 1.6
  // Устанавливаем scrollFactor в 0, чтобы фон не двигался при скролле
  bgImage.setScrollFactor(0)
  // Перемещаем фон в самый низ
  bgImage.y = this.scale.height - bgImage.displayHeight
  // Спавним воздушный шар и ставим пивот по центру
  const balloon = this.add
    .image(this.scale.width / 2, this.scale.height / 2, 'balloon')
    .setOrigin(0.5, 0.5)
  // Шрина и высота
  balloon.displayWidth = 450
  balloon.displayHeight = 750
  // Позиция
  balloon.y = this.scale.height - 560
}

function update() {
  // тут обновляешь игровой мир
}
</script>

<style>
.game-container {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden;
  position: relative; /* обязательно для absolute элементов HUD */
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
  z-index: 10;
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
