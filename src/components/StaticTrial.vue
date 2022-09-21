<template>
  <div class="screen">
    <div id="target"
      :style="{ 'left' : x, 'top' : y }"
    ></div>
  </div>
</template>

<script>
export default {
  name: 'StaticTrial',
  props: {
    x: String, 
    y: String
  },
  methods: {
    endTrial() {
      this.$emit('finish');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.screen {
  width: 100%;
  height: 100%;
  background-color: black;
  position: relative;
}

#target {
  min-height: 5px;
  min-width: 5px;
  position: absolute;
  background-color: white;
}
</style>
