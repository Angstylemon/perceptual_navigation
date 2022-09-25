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
    <component v-else 
      :is="trialType"
      :duration="trialDuration"
      :activationRange="activationRange"
      v-on:finish="endTrial"
      v-on:activate="activate"
    ></component>
    <!-- <CircleMovingTrial
      v-else
      :duration="20"
      v-on:finish="endTrial"
      v-on:activate="activate"
    /> -->
    <!-- <StaticTrial
      v-else
      :duration="10"
      v-on:finish="endTrial"
      v-on:activate="activate"
    /> -->
  </div>
</template>

<script>
import StartButton from '../components/StartButton.vue';
import StaticTrial from '../components/StaticTrial.vue';
import CircleMovingTrial from '../components/CircleMovingTrial.vue';
import SquareMovingTrial from '../components/SquareMovingTrial.vue';

export default {
  name: 'Home',
  components: {
    StartButton,
    StaticTrial,
    CircleMovingTrial,
    SquareMovingTrial
},
  data() { 
    return {
      start: false,
      trialType: "SquareMovingTrial",
      startX: "50%",
      startY: "50%",
      activationValue: 0,
      activationRange: 150,
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
    },  
  },
  computed: {
    trialDuration() {
      let duration = 0;

      if (this.trialType == "StaticTrial") {
        duration = 10;
      } else if (this.trialType == "CircleMovingTrial") {
        duration = 20;
      } else if (this.trialType == "SquareMovingTrial") {
        duration = 40;
      }

      return duration;
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