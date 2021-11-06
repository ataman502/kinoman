<template>
  <div class="loader__container">
    <div
      class="loader-roller"
      :class="color"
    >
      <div />
      <div />
      <div />
      <div />
      <div />
      <div />
      <div />
      <div />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Loader',
  computed: {
    color () {
      const colors = ['green', 'red', 'yellow']
      return colors[Math.floor(Math.random() * 3)]
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/style/vars/_vars';

.loader__container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
