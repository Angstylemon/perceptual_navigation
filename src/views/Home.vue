<template>
  <div class="screen">
    <StartButton
      v-if="!start"
      x="50%" 
      y="50%"
      v-on:click="startTrial"
    />
    <StaticTrial
      v-else
      x="0px"
      y="0px"
      v-on:finish="endTrial"
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
    },
    endTrial() {
      this.start = false;
    }
  }
}
</script>

<style lang="scss" scoped>
.screen {
  background-color: black;
  position: absolute;
  width: 100%;
  height: 100%;
  max-width: 600px;
  max-height: 600px;
  
  flex: 1 0 100%;
  display: flex;
}
</style>