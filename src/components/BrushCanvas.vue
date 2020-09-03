<template>
  <div class="rect-canvas">
    <canvas :id="canvasId" class="canvas-style" @mousedown="mouseDown" />
  </div>
</template>

<script>
const paper = require("paper");

export default {
  name: "BrushCanvas",
  props: ["canvasId"],
  data: () => ({
    path: null,
    scope: null,
  }),
  methods: {
    reset() {
      this.scope.project.activeLayer.removeChildren();
    },
    pathCreate(scope) {
      scope.activate();
      return new paper.Path({
        strokeColor: "white",
        strokeJoin: "round",
        strokeWidth: 20,
        strokeCap: 'round',
      });
    },
    createTool(scope) {
      scope.activate();
      return new paper.Tool();
    },
    mouseDown() {
      // in order to access functions in nested tool
      let self = this;
      // create drawing tool
      this.tool = this.createTool(this.scope);
      this.tool.onMouseDown = (event) => {
        // init path
        self.path = self.pathCreate(self.scope);
        // add point to path
        self.path.add(event.point);
      };
      this.tool.onMouseDrag = (event) => {
        self.path.add(event);
      };
      this.tool.onMouseUp = (event) => {
        // line completed
        self.path.add(event.point);
      };
    },
  },
  mounted() {
    this.scope = new paper.PaperScope();
    this.scope.setup(this.canvasId);
  },
};
</script>

<style scoped>
.rect-canvas {
  position: absolute;
  top: 0;
  z-index: 20;
}
.canvas-style {
  cursor: grab;
  width: 600px !important;
  height: 600px !important;
  border: 1px solid rgb(192, 192, 192);
  display: block;
}
</style>
