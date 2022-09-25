<template>
  <div class="screen"
    :style="{ 'background-color' : start ? `rgb(${activationValue * 255}, 0, 0)` : 'black' }"  
  >
    <StartButton
      v-if="!start"
      :x="startX" 
      :y="startY"
      v-on:click="startTrial"
    />
    <StaticTrial
      v-else
      :duration="10"
      v-on:finish="endTrial"
      v-on:activate="activate"
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
      activationValue: 0,
    }
  },
  methods: {
    startTrial() {
      this.activationValue = 0;
      this.start = true;
    },
    endTrial() {
      this.start = false;
    },
    activate(x) {
      // tanh prime activation                                
      const z = x * 2.5;                                                       
                                                                                  
      const tanh = (Math.exp(z) - Math.exp(-z)) / (Math.exp(z) + Math.exp(-z));
      const tanhPrime = 1 - Math.pow(tanh, 2);                                
                                                                                  
      this.activationValue = tanhPrime;

      console.log(this.activationValue);

    },  
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