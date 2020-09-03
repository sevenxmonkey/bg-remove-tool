<template>
  <div class="img-canvas">
    <div class="img-container" :style="{'clip-path': clipPath}">
      <img
        :src="image"
        :style="{
        width:`${width}px`, 
        left: `${left}px`, 
        top: `${top}px`} "
      />
    </div>
    <div v-if="mode === 'rect'" class="img-container-2" style="opacity:0.5">
      <img
        :src="image"
        :style="{
        width:`${width}px`, 
        left: `${left}px`, 
        top: `${top}px`, 
        position:'absolute'} "
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "ImageUpload",
  data() {
    return {
      image: false,
      width: 600,
      left: 0,
      top: 0,
      removebgAPI_KEY: "pHtR4KdPy6uavhUDkvtJSRZh",
      clipPath: "",
      mode: "rect"
    };
  },
  methods: {
    createImage(file) {
      const self = this;
      var img = new Image();
      var reader = new FileReader();

      reader.onload = (e) => {
        self.image = e.target.result;
        img.src = e.target.result;
      };

      img.onload = () => {
        self.computeWidth(img.width, img.height);
      };
      reader.readAsDataURL(file);
    },
    removeImage: function () {
      this.image = false;
    },
    computeWidth(w, h) {
      if (w < h) {
        this.width = (600 * w) / h;
        this.left = (600 - this.width) / 2;
        this.top = 0;
      } else {
        this.width = 600;
        this.left = 0;
        this.top = 300 * (1 - h / w);
      }
    },
  },
};
</script>

<style scoped>
.img-canvas {
  width: 600px;
  height: 600px;
  position: absolute;
  z-index: 1;
  overflow: hidden;
}
.img-container {
  width: 100%;
  height: 100%;
  background-color: rgb(255, 255, 255);
}
img {
  cursor: crosshair;
  width: 600px;
  display: block;
  position: relative;
}
</style>