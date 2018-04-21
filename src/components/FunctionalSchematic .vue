<template>
<el-carousel :interval="4000" type="card" height="300px">
<el-carousel-item v-for="item in img" :key="item.pic">
<img :src="item.src" alt="">
</el-carousel-item>
</el-carousel>
</template>
<script>
  export default {
    data() {
      return {
        img: [
          {'pic': '01', 'src': require('../assets/1.jpg')},
          {'pic': '02', 'src': require('../assets/1.jpg')},
          {'pic': '03', 'src': require('../assets/1.jpg')}
        ]
      }
    }
  }
</script>
