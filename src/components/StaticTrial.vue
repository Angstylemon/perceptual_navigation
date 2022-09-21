<template>
  <div class="trial">
    <div id="target"
      ref="target"
      :style="{ 'left' : x, 'top' : y }"
    ></div>
  </div>
</template>

<script>
export default {
  name: 'StaticTrial',
  props: {
    x: String, 
    y: String,
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
    }
  },
  mounted() {
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
      // this.$emit('finish');
    },
    mousemoveHandler(event) {
      this.mouseX = event.clientX;
      this.mouseY = event.clientY;
    },
    animationFrame() {
      let target = this.$refs['target'].getBoundingClientRect();
      let distance_to_target = this.calculateDistance({x: target.x, y: target.y}, {x: this.mouseX, y: this.mouseY});

      console.log(distance_to_target);
    },
    calculateDistance(p1, p2) {
      let x_diff = p2.x - p1.x;
      let y_diff = p2.y - p1.y;

      let sqaure_diff = x_diff * x_diff + y_diff * y_diff;
      let diff = Math.sqrt(sqaure_diff);

      return diff;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.trial {
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
