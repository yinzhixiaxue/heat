<template>
  <el-container>
    <el-header>
      供暖收费管理系统
      <div class="header-info">
        <div v-if="true">
          <span>{{name}}已登录</span>
          <router-link to="/login" @click.native="layout">注销</router-link> 
        </div>
        <div v-else>
          <span>未登录</span>
          <a>请先登陆</a>
        </div>
      </div>
    </el-header>
  </el-container>
</template>
<script>
  export default {
    created() {
      console.log(localStorage.username);
      if (localStorage.username) {
        this.name = localStorage.username;
        this.flag = true;
      } else {
        this.flag = false;
      }
    },
    data() {
      return {
        name: '你好',
        flag: false,
      }
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
      layout: function () {
        localStorage.clear();
        this.$router.push('./login');
      }
    }
  }
</script>
