<template>
  <el-container>
    <el-header>
      供暖收费管理系统
      <div class="header-info">
        <div v-if="true">
          <span>{{name}}已登录</span>
          <a>退出登陆</a>
        </div>
        <div v-else>
          <span>未登录</span>
          <a>请先登陆</a>
        </div>
      </div>
    </el-header>
    <el-main>
      <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
        <el-menu-item index="1">客户管理</el-menu-item>
        <el-menu-item index="2">收费管理</el-menu-item>
        <el-menu-item index="3">清欠管理</el-menu-item>
        <el-menu-item index="4"><a href="https://www.ele.me" target="_blank">变更管理</a></el-menu-item>
        <el-menu-item index="5">减免折让</el-menu-item>
      </el-menu>

      <el-carousel :interval="4000" type="card" height="300px">
        <el-carousel-item :key="index" v-for="(src,index) in img">
          <img :src="src" alt="">
        </el-carousel-item>
      </el-carousel>
    </el-main>
    <el-footer>版权所有权@杨昊东</el-footer>
  </el-container>
</template>
<script>
  export default {
    name: 'Home',
    data() {
      return {
        name: '你好',
        activeIndex: '1',
        activeIndex2: '1',
        img: [
          "src/assets/1.jpg",
          "src/assets/1.jpg",
          "src/assets/1.jpg",
          "src/assets/1.jpg"
        ]
      }
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      }
    }
  }
</script>
<style scoped>
  a{
    text-decoration: none;
  }
  .el-carousel {
    overflow: hidden;
    width: 960px;
    margin: 0 auto;
    padding-top: 100px;
    min-height: 350px;
    /*background: #324057;*/
  }

  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin-top: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
  .el-carousel__item img {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    width: 100%;
    height: 300px;
  }

  .header-info {
    float: right;
    height: 60px;
    overflow: hidden;
    font-size: 2px;
  }

  .header-info div {
    display: inline;
    cursor: pointer;
  }

  .header-info a:hover {
    color: #409EFF;
  }

  .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }

  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
    height: 650px;
  }

  body > .el-container {
    margin-bottom: 40px;
  }

  .el-container:nth-child(5) .el-aside,
  .el-container:nth-child(6) .el-aside {
    line-height: 260px;
  }

  .el-container:nth-child(7) .el-aside {
    line-height: 320px;
  }
</style>
