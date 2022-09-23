<template>
  <div class="screen">
    <StartButton
      v-if="!start"
      :x="startX" 
      :y="startY"
      v-on:click="startTrial"
    />
    <StaticTrial
      v-else
      duration="10"
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
      startX: "50%",
      startY: "50%",
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