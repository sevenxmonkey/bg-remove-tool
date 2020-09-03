<template>
  <div class="img-canvas">
    <img :src="image" :style="{width:`${width}px`, left: `${left}px`, top: `${top}px`}" />
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
    };
  },
  methods: {
    createImage(file, event) {
      const self = this;
      var img = new Image();
      var reader = new FileReader();

      console.log(file);
      console.log(event);

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
        this.top = 300 * (1 - h / w)
      }
    },
  },
};
</script>

<style scoped>
.img-canvas {
  background-color: rgb(255, 255, 255);
  width: 600px;
  height: 600px;
  position: absolute;
  z-index: 1;
  overflow: hidden;
}

img {
  width: 600px;
  display: block;
  position: relative;
}
</style>