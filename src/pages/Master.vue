<template>
  <div>
    <div>
      <Canvas :canvas-id="'canvas-one'" ref="canvas1" @finishOneRect="handleFinish" />
    </div>
    <div>
      <a-button @click.prevent="reset">Clear</a-button>
    </div>
  </div>
</template>

<script>
import Canvas from "../components/Canvas";
export default {
  name: "Master",
  components: {
    Canvas,
  },
  data(){
    return{
      rects: []
    }
  },
  methods: {
    reset() {
      this.$refs.canvas1.reset();
      this.rects = [];
    },
    handleFinish(cor){
      this.rects.push(cor)
      console.log(this.rects)
    }
  },
};
</script>

<style scoped>
</style>
