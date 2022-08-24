<template>
  <!-- prettier-ignore -->
  <div class="text-left mt-10">
    <p class="font-medium text-xl">Take the quiz</p>
    <p class="font-medium text-xl">and try your firs pair</p>
    <slot> </slot>
    <p class="ml-1">30 days risk free</p>
    <img class="start-screen-image absolute bottom-0" src="../assets/ImageStartScreen.png" alt="">
  </div>
</template>

<script>
export default {};
</script>

<style>
.start-screen-image {
  height: 63%;
  width: 100%;
}
</style>
