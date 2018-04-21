<template>
  <div>
    <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
      <el-menu-item index="1" @click="toggleTab('FunctionalSchematic')">功能示意图</el-menu-item>
      <el-menu-item index="2" @click="toggleTab('charge')">客户管理</el-menu-item>
      <el-menu-item index="3" @click="toggleTab('charge')">收费管理</el-menu-item>
      <el-menu-item index="4" @click="toggleTab('Debt')">清欠管理</el-menu-item>
      <el-menu-item index="5" @click="toggleTab('Change')">变更管理</el-menu-item>
      <el-menu-item index="6" @click="toggleTab('Reduction')">减免折让</el-menu-item>
    </el-menu>
    <div class="main-content" keep-alive="true">
      <charge  :is="currentTab" ></charge>
    </div>
  </div>

</template>
<script>
  import FunctionalSchematic from './FunctionalSchematic .vue'
  import charge from './charge'
  import Change from './ClientChange.vue'
  import Reduction from './Reduction.vue'
  import Debt from './Debt.vue'

  export default {
    data() {
      return {
      	activeIndex: '1',
        currentTab: 'FunctionalSchematic'
      }
    },
    components: {
      FunctionalSchematic,
       charge,
       Change,
      Reduction,
      Debt
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      }
      ,
        toggleTab: function(tab) {
          this.currentTab = tab; // tab 为当前触发标签页的组件名
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
  .main-content{
    height: 600px;
  }
</style>
