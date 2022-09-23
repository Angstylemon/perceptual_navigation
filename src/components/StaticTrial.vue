<template>
  <div 
    class="background"
    :style="{ 'background-color' : `rgb(${activationValue * 255}, 0, 0)` }"  
  >
    <div 
      class="target"
      ref="target"
      :style="{ 'left' : targetX, 'top' : targetY }"
    ></div>
  </div>
</template>

<script>
export default {
  name: 'StaticTrial',
  props: {
    duration: {
      type: Number,
      default: 1
    },
    activationRange: {
      type: Number,
      default: 200,
    }
  },
  data() {
    return {
      mouseX: 0,
      mouseY: 0,
      animationCycle: null,
      targetX: 0,
      targetY: 0,
      activationValue: 0,
    }
  },
  mounted() {
    // Generate target position
    const border = 0.15;
    this.targetX = this.generateRandomOffset(border)+"%";
    this.targetY = this.generateRandomOffset(border)+"%";

    // Setup trial functionality
    document.addEventListener('mousemove', this.mousemoveHandler);
    
    let framerate = 1000/60;
    this.animationCycle = setInterval(this.animationFrame, framerate);
  
    // Timer to end trial
    let trial_duration_millisec = 1000 * this.duration;
    setTimeout(this.endTrial, trial_duration_millisec);
  },
  beforeDestroy() {
    clearInterval(this.animationCycle);
  },
  methods: {
    endTrial() {
      this.$emit('finish');
    },
    mousemoveHandler(event) {
      this.mouseX = event.clientX;
      this.mouseY = event.clientY;
    },
    animationFrame() {
      const target = this.$refs['target'].getBoundingClientRect();
      const distance_to_target = this.calculateDistance({x: target.x, y: target.y}, {x: this.mouseX, y: this.mouseY});
      const relative_distance = distance_to_target/this.activationRange;

      this.activationValue = this.activate(relative_distance);
    },
    activate(x) {
      // tanh prime activation                                
      const z = x * 2.5;                                                       
                                                                                  
      const tanh = (Math.exp(z) - Math.exp(-z)) / (Math.exp(z) + Math.exp(-z));
      const tanh_prime = 1 - Math.pow(tanh, 2);                                
                                                                                  
      return tanh_prime;                                                 
    },                                                                          
    calculateDistance(p1, p2) {
      let x_diff = p2.x - p1.x;
      let y_diff = p2.y - p1.y;

      let sqaure_diff = x_diff * x_diff + y_diff * y_diff;
      let diff = Math.sqrt(sqaure_diff);

      return diff;
    },
    generateRandomOffset(border) {
      const random_val = Math.random();
      const random_within_range = random_val * (1 - 2*border);
      const random_with_offset = random_within_range + border;
      
      let signed_random = random_with_offset;

      if (Math.random() > 0.5) {
        signed_random *= -1;
      }

      // Offset is from centre of screen, i.e. 50%
      // signed_random contains a scaled random value
      // and is either positive or negative
      const final_offset = 50*(1 + signed_random);

      return final_offset;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.background {
  width: 100%;
  height: 100%;
  background-color: black;
  position: relative;
}

.target {
  visibility: hidden;
  min-height: 5px;
  min-width: 5px;
  position: absolute;
  transform: translate(-50%, -50%);
  background-color: white;
}
</style>
