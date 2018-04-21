<template>
  <div>
    <v-home></v-home>
    <v-nav></v-nav>
    <v-footer></v-footer>
  </div>
</template>
<script>

  import Home from './Header'
  import Nav from './NavMain'
  import Footer from './Footer'
  export default {
    components: {
      'v-nav': Nav,
      'v-home': Home,
      'v-footer': Footer,
    },
    data() {
      return {
        flag: false,
      }
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
    }
  }
</script>
<style scoped>
  a {
    text-decoration: none;
  }

  .el-carousel {
    overflow: hidden;
    width: 960px;
    margin: 0 auto;
    padding-top: 100px;
    min-height: 500px;
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
