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
  </el-container>
</template>