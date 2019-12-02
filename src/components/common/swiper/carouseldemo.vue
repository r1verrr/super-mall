<template>
  <div>
    <el-carousel height="500px">
      <el-carousel-item v-for="item in pics" :key="item.url">
        <img :src="item.url"/>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pics: [
        { url: require("assets/pic1.jpg") },
        { url: require("assets/pic2.jpg") },
        { url: require("assets/pic3.jpg") },
        { url: require("assets/pic4.jpg") }
      ]
    };
  }
};
</script>