<template>
  <div 
    class="start" 
    :style="{
      'left': x,
      'top': y
    }"
  >
    <button 
      class="start__btn"
      @click="clickHandler()"
    >Click</button>
  </div>
</template>

<script>
export default {
  name: 'StartButton',
  props: ['x', 'y'],
  methods: {
    clickHandler() {
      this.$emit('click');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.start {
  position: absolute;
  transform: translate(-50%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;
  height: fit-content;
  width: fit-content;

  &__btn {
    background-color: red;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    color: black;
    font-size: 0.875rem;
  }
}
</style>
