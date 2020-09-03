<template>
  <div class="workplace">
    <div>
      <a-button>
        <input type="file" @change="onFileChange($event)" />
      </a-button>
      <a-button @click.prevent="clear">Clear</a-button>
      <a-button @click.prevent="adjust">Adjust</a-button>
    </div>
    <div style="position: relative">
      <ImageUpload ref="imgcanvas" />
      <BrushCanvas v-if="mode === 'brush'" :canvas-id="'canvas-two'" ref="brushcanvas" />
      <RectCanvas
        :canvas-id="'canvas-one'"
        ref="rectcanvas"
        @finishOneRect="handleFinish"
        @beginRect="handleBeginRect"
      />
    </div>
  </div>
</template>

<script>
import RectCanvas from "../components/RectCanvas";
import ImageUpload from "../components/ImageUpload";
import BrushCanvas from "../components/BrushCanvas";

export default {
  name: "Master",
  components: {
    RectCanvas,
    ImageUpload,
    BrushCanvas,
  },
  data() {
    return {
      mask: {
        rects: [],
        brush: [],
      },
      hasImage: false,
      image: null,
      mode: "rect",
    };
  },
  methods: {
    onFileChange(e) {
      this.mode === "rect"
      this.rects = [];
      this.$refs.rectcanvas.reset();
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.$refs.imgcanvas.createImage(files[0]);
    },
    clear() {
      if (this.mode === "rect") {
        this.$refs.rectcanvas.reset();
        this.mask.rects = [];
        this.$refs.imgcanvas.clipPath = "";
      }else{
        this.$refs.brushcanvas.reset();
      }
    },
    handleBeginRect() {
      this.$refs.imgcanvas.clipPath = "";
    },
    handleFinish(cor) {
      this.mask.rects = cor;
      // console.log(this.mask);
      const clipPath = this.computePer(cor);
      this.$refs.imgcanvas.clipPath = clipPath;
    },
    computePer(cor) {
      const x1 = Math.min(cor[0][0], cor[1][0]);
      const x2 = Math.max(cor[0][0], cor[1][0]);
      const y1 = Math.min(cor[0][1], cor[1][1]);
      const y2 = Math.max(cor[0][1], cor[1][1]);

      const a = String((y1 / 600) * 100);
      const b = String((1 - x2 / 600) * 100);
      const c = String((1 - y2 / 600) * 100);
      const d = String((x1 / 600) * 100);

      return `inset(${a}% ${b}% ${c}% ${d}%)`;
    },
    setImage: function (file) {
      this.hasImage = true;
      this.image = file;
      console.log(file);
    },
    adjust() {
      this.$refs.imgcanvas.mode = "brush";
      this.$refs.rectcanvas.reset();
      this.mode = "brush";
    },
  },
};
</script>

<style scoped>
.workplace {
  width: 600px;
  margin: auto;
}
</style>
