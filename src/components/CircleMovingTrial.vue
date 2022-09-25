<template>
  <div 
    class="target"
    ref="target"
    :style="{ 
      'left' : `${targetX}%`, 
      'top' : `${targetY}%`,
      'transform-origin' : `${xOffset}px ${yOffset}px`
    }"
  ></div>
</template>

<script>
export default {
  name: 'CircleMovingTrial',
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
      xOffset: 0,
      yOffset: 0,
    }
  },
  mounted() {
    // Generate target position
    const border = 0.25;
    this.targetX = this.generateRandomOffset(border);
    this.targetY = this.generateRandomOffset(border);
    this.translateOrigin = this.calculateOrigin();

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
      const distanceToTarget = this.calculateDistance({x: target.x, y: target.y}, {x: this.mouseX, y: this.mouseY});
      const relativeDistance = distanceToTarget/this.activationRange;

      this.$emit('activate', relativeDistance);
    },                                                                       
    calculateDistance(p1, p2) {
      let x_diff = p2.x - p1.x;
      let y_diff = p2.y - p1.y;

      let sqaureDiff = x_diff * x_diff + y_diff * y_diff;
      let diff = Math.sqrt(sqaureDiff);

      return diff;
    },
    generateRandomOffset(border) {
      const randomVal = Math.random();
      const randomWithinRange = randomVal * (1 - 2*border);
      const randomWithOffset = randomWithinRange + border;
      
      let signedRandom = randomWithOffset;

      if (Math.random() > 0.5) {
        signedRandom *= -1;
      }

      // Offset is from centre of screen, i.e. 50%
      // signed_random contains a scaled random value
      // and is either positive or negative
      const finalOffset = 50*(1 + signedRandom);

      return finalOffset;
    },
    calculateOrigin() {
      const height = this.$parent.$el.scrollHeight;
      const width = this.$parent.$el.scrollWidth;

      const xCentre = width * 0.5;
      const yCentre = height * 0.5;

      const xTarget = width * this.targetX/100;
      const yTarget = height * this.targetY/100;

      const xDiff = xCentre - xTarget;
      const yDiff = yCentre - yTarget;

      this.xOffset = xDiff;
      this.yOffset = yDiff;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.target {
  visibility: hidden;
  min-height: 5px;
  min-width: 5px;
  position: absolute;
  transform: translate(-50%, -50%);
  background-color: white;

  animation-name: target-animate;
  animation-duration: 15s;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
  transform-origin: 50% 50%;
}

@keyframes target-animate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(359deg);
  }
}
</style>
