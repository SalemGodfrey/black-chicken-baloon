<template>
  <div class="balance-box-container">
    <div ref="balanceBox" class="balance-box">
      <span class="balance-box-title">BALANCE</span>
      <span class="balance-box-amount"
        >{{ balanceValue }}
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
      </span>
    </div>
  </div>
</template>

<script setup>

// import { ref, onMounted } from 'vue'

defineProps({
  balanceValue: Number
})
</script>

<style>
.balance-box-container {
  position: absolute;
  top: 0;
  left: 50%;
  width: 100%;
  padding: 0 10px;
  z-index: 3;
  transform: translateX(-50%);
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.balance-box {
  margin-left: 9.5rem;
  padding: 10px 15px 10px 15px;
  background-color: #1c1c21;
  border-radius: 10px;
  color: #c8c8c9;
  z-index: 10;
  font-family: 'Dela Gothic One' !important;
  width: 100%;
  max-width: 30rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 1.2rem;
}

.balance-box-amount {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  width: fit-content;
}

@media screen and (max-width: 662px) {
  .balance-box {
    border-radius: 10px 0 0 10px;
  }
}

@media screen and (max-width: 500px) {
  .balance-box {
    max-width: 100%;
    width: 100%;
    margin-left: 0;
    margin-bottom: 20px;
    border-radius: 0;
  }
}
</style>
