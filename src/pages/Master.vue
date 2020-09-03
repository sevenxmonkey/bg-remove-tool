<template>
  <div class="workplace">
    <ImageUpload ref="imgcanvas" />
    <Canvas :canvas-id="'canvas-one'" ref="rectcanvas" @finishOneRect="handleFinish" />
    <div>
      <a-button>
        <input type="file" @change="onFileChange($event)" />
      </a-button>
      <a-button @click.prevent="resetRect">Clear</a-button>
      <a-button @click.prevent="adjust">Adjust</a-button>
    </div>
  </div>
</template>

<script>
import Canvas from "../components/Canvas";
import ImageUpload from "../components/ImageUpload";

export default {
  name: "Master",
  components: {
    Canvas,
    ImageUpload,
  },
  data() {
    return {
      rects: [],
      hasImage: false,
      image: null,
      mode: "adjust"
    };
  },
  methods: {
    onFileChange(e) {
      this.rects = [];
      this.$refs.rectcanvas.reset();
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.$refs.imgcanvas.createImage(files[0], e);
    },
    resetRect() {
      this.$refs.rectcanvas.reset();
      this.rects = [];
    },
    handleFinish(cor) {
      this.rects.push(cor);
      console.log(this.rects);
    },
    setImage: function (file) {
      this.hasImage = true;
      this.image = file;
      console.log(file);
    },
    adjust(){
      // console.log(this.$refs.imgcanvas.image)
      console.log(this.rects);
    }
  },
};
</script>

<style scoped>
.workplace {
  width: 600px;
  margin: auto;
}
</style>
