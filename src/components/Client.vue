<template>
	<div>
	  <el-container>
	    <el-header>
	      供暖收费管理系统
	      <div class="header-info">
	        <div v-if="true">
	          <span>{{name}}已登录</span>
	          <a @click ='layout'>退出登陆</a>
	          <!-- <router-link to="/Login">退出登陆</router-link> -->
	        </div>
	        <div v-else>
	          <span>未登录</span>
	          <a>请先登陆</a>
	        </div>
	      </div>
	    </el-header>
	    <v-nav></v-nav>
	    <el-button type="text" @click="centerDialogVisible = true">新建用户</el-button>

		<el-dialog
		  title="请完善用户信息"
		  :visible.sync="centerDialogVisible"
		  width="50%"
		  center>
		  <!-- <span>需要注意的是内容是默认不居中的</span> -->
		  
		  <span slot="footer" class="dialog-footer">
		    <el-button @click="centerDialogVisible = false">取 消</el-button>
		    <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
		  </span>
		</el-dialog>
	  </el-container>
	</div>
</template>
<script>
import nav from './Nav'
  export default {
    components:{
          'v-nav': nav
      },
    data() {
      return {
        name: '你好',
        centerDialogVisible: false
      }
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
      layout: function() {
        localStorage.clear();
        this.$router.push('./login');
      } 
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