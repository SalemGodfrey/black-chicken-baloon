<template>
  <video playsinline muted autoplay v-show="props.isCockUp" loop class="cock idle">
    <source src="@/assets/cock.webm" type="video/webm" />
  </video>
  <video autoplay playsinline muted v-show="!props.isCockUp" class="cock dead-cock">
    <source src="@/assets/cock-up.webm" type="video/webm" />
  </video>
  <video autoplay playsinline muted v-if="props.isCockDead" class="explode">
    <source src="@/assets/EXPLOSION.webm" type="video/webm" />
  </video>
</template>

<script setup>
const props = defineProps({
  isCockUp: {
    type: Boolean,
    default: false,
  },
  isCockDead: {
    type: Boolean,
    default: true,
  },
})
</script>

<style>
.cock, .idle {
  position: absolute;
  bottom: 12rem;
  left: 50%;
  transform: translate(-50%);
  z-index: 1;
  width: 30rem;
  max-width: 39rem;
}
.explode {
  position: absolute;
  bottom: 18rem;
  left: 50%;
  transform: translate(-50%);
  z-index: 1;
  width: 20rem;
}
</style>
