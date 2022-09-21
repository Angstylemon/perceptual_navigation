<template>
  <div id="screen">
    <StartButton
      v-if="!start"
      x="50%" 
      y="50%"
      v-on:click="startTrial"
    />
    <StaticTrial
      v-else
      x="200px"
      y="200px"
    />
  </div>
</template>

<script>
import StartButton from '../components/StartButton.vue';
import StaticTrial from '../components/StaticTrial.vue';

export default {
  name: 'Home',
  components: {
    StartButton,
    StaticTrial
  },
  data() { 
    return {
      start: false,
    }
  },
  methods: {
    startTrial() {
      this.start = true;
    }
  }
}
</script>

<style lang="scss" scoped>
#screen {
  width: 600px;
  height: 600px;
  background-color: black;
  position: relative;
}
</style>