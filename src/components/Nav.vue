<template>
	<el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
        <el-menu-item index="1">功能示意图</el-menu-item>
        <el-menu-item index="2"><router-link to="/Client">客户管理</router-link></el-menu-item>
        <el-menu-item index="3">收费管理</el-menu-item>
        <el-menu-item index="4">清欠管理</el-menu-item>
        <el-menu-item index="5"><a href="https://www.ele.me" target="_blank">变更管理</a></el-menu-item>
        <el-menu-item index="6">减免折让</el-menu-item>
     </el-menu>
</template>
<script>
	export default {
    data() {
      return {
      	activeIndex: '1',
      }
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      }
     }
  }
</script>
<style scope>
	a {
		display: inline-block;
		text-decoration: none;
		/*width: 100%;*/
	}
</style>