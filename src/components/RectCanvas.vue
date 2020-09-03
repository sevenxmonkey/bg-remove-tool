<template>
  <div class="rect-canvas">
    <canvas :id="canvasId" class="canvas-style" @mousedown="mouseDown" />
  </div>
</template>

<script>
const paper = require("paper");

export default {
  name: "RectCanvas",
  props: ["canvasId"],
  data: () => ({
    paths: null,
    scope: null,
    from: null,
    array: [],
  }),
  methods: {
    reset() {
      
      this.scope.project.activeLayer.removeChildren();
    },
    createTool(scope) {
      scope.activate();
      return new paper.Tool();
    },
    createRect(from, to, scope) {
      scope.activate();

      var rect = new paper.Shape.Rectangle(from, to);
      rect.strokeColor = "#F2F2F2";
      rect.strokeWidth = 3;
      return rect;
    },
    mouseDown() {
      this.reset();
      this.beginRect();

      let self = this;
      // create drawing tool
      this.tool = this.createTool(this.scope);
      this.tool.onMouseDown = (event) => {
        self.from = event.point;
        self.path = self.createRect(self.from, event.point, self.scope);
      };
      this.tool.onMouseDrag = (event) => {
        self.to = event.point;
        const weight = self.to.y - self.from.y;
        const height = self.to.x - self.from.x;
        const curSz = new paper.Size(height, weight);
        self.path.size = curSz;

        const dirs = ["topLeft", "bottomLeft", "topRight", "bottomRight"];
        let dirTo = "";
        let dim1 = self.to.x - self.from.x;
        let dim2 = self.to.y - self.from.y;
        if (dim1 <= 0 && dim2 <= 0) {
          dirTo = dirs[0];
        } else if (dim1 <= 0 && dim2 > 0) {
          dirTo = dirs[1];
        } else if (dim1 > 0 && dim2 < 0) {
          dirTo = dirs[2];
        } else {
          dirTo = dirs[3];
        }
        self.path.bounds[dirTo] = self.to;
      };
      this.tool.onMouseUp = (event) => {
        self.to = event.point;
        if (self.to.x !== self.from.x && self.to.y !== self.from.y) {
          self.finishRect();
        }
      };
    },
    finishRect() {
      this.$emit("finishOneRect", [
        [this.from.x, this.from.y],
        [this.to.x, this.to.y],
      ]);
    },
    beginRect(){
      this.$emit("beginRect")
    }
  },
  mounted() {
    this.scope = new paper.PaperScope();
    this.scope.setup(this.canvasId);
  },
};
</script>

<style scoped>
.rect-canvas {
  position: relative; 
  z-index: 10
}
.canvas-style {
  cursor: crosshair;
  width: 600px !important;
  height: 600px !important;
  border: 1px solid rgb(192, 192, 192);
  display: block;
  
}
</style>
