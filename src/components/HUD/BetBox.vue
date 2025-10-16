<template>
  <div class="bet-panel">
    <div class="bet-info">
      <div class="bet-label">BET</div>
      <div class="bet-amount">
        <svg
          fill="#ffffff"
          height="25px"
          width="25px"
          version="1.1"
          id="Layer_1"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          viewBox="0 0 512 512"
          xml:space="preserve"
          stroke="#ffffff"
        >
          <g id="SVGRepo_iconCarrier">
            <g>
              <g>
                <path
                  d="M256,0C114.837,0,0,114.843,0,256s114.837,256,256,256s256-114.843,256-256S397.163,0,256,0z M256,478.609 c-122.75,0-222.609-99.865-222.609-222.609S133.25,33.391,256,33.391S478.609,133.256,478.609,256S378.75,478.609,256,478.609z"
                ></path>
              </g>
            </g>
            <g>
              <g>
                <path
                  d="M256,66.783C151.663,66.783,66.783,151.663,66.783,256S151.663,445.217,256,445.217S445.217,360.337,445.217,256 S360.337,66.783,256,66.783z M263.805,241.244c23.794,12.576,53.413,28.233,53.413,59.277c0,27.961-18.869,51.551-44.522,58.843 v7.939c0,9.223-7.479,16.696-16.696,16.696c-9.217,0-16.696-7.473-16.696-16.696v-7.939 c-25.652-7.293-44.522-30.883-44.522-58.843c0-9.223,7.479-16.696,16.696-16.696s16.696,7.473,16.696,16.696 c0,15.342,12.478,27.826,27.826,27.826s27.826-12.484,27.826-27.826c0-10.923-18.109-20.5-35.631-29.755 c-23.794-12.582-53.413-28.239-53.413-59.288c0-27.961,18.869-51.551,44.522-58.843v-7.939c0-9.223,7.479-16.696,16.696-16.696 c9.217,0,16.696,7.473,16.696,16.696v7.939c25.652,7.293,44.522,30.883,44.522,58.843c0,9.223-7.479,16.696-16.696,16.696 s-16.696-7.473-16.696-16.696c0-15.342-12.478-27.826-27.826-27.826s-27.826,12.484-27.826,27.826 C228.174,222.407,246.283,231.984,263.805,241.244z"
                ></path>
              </g>
            </g>
          </g>
        </svg>
        <span id="betValue">{{ betValue }}</span>
      </div>
    </div>
    <div class="controls">
      <div class="control-btn" @click="increaseBet()">
        <div class="arrow arrow-up"></div>
      </div>
      <div class="control-btn" @click="decreaseBet()">
        <div class="arrow arrow-down"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const betValue = ref(1)

const emit = defineEmits(['betRef'])

onMounted(() => {
  emit('betRef', betValue.value)
})

function increaseBet() {
  betValue.value++
}
function decreaseBet() {
  betValue.value--
}
</script>

<style>
.bet-panel {
  background: #212226;
  border: 2px solid #37383b;
  border-radius: 10px;
  padding: 5px 10px;
  width: 100%;
  max-width: 13rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 30px;
  font-family: 'Dela Gothic One';
}

.bet-info {
  display: flex;
  flex-direction: column;
  /* gap: 2px; */
}

.bet-label {
  color: #ffffff;
  font-size: 20px;
  font-weight: 900;
  letter-spacing: 2px;
  text-transform: uppercase;
}

#betValue {
  transform: translateY(-2px);
}

.bet-amount {
  color: #ffffff;
  font-size: 22px;
  display: flex;
  align-items: center;
  gap: 5px;
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.control-btn {
  width: 40px;
  height: 25px;
  background: #3a3a3a;
  border: 2px solid #505050;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s;
}

.control-btn:hover {
  background: #454545;
  border-color: #606060;
}

.control-btn:active {
  transform: scale(0.95);
}

.arrow {
  width: 0;
  height: 0;
  border-left: 7px solid transparent;
  border-right: 7px solid transparent;
}

.arrow-up {
  border-bottom: 8px solid #ffffff;
}

.arrow-down {
  border-top: 8px solid #ffffff;
}
</style>
