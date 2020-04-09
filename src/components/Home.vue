<template>
  <el-container class="home_container">
    <!-- 头部 -->
    <el-header>
      <div>
        <img src alt />
        <span>云存储系统</span>
      </div>
      <el-button type="info" @click="Logout">退出</el-button>
    </el-header>
    <!-- 页面主体 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside width="200px">
        <el-menu background-color="#2cd5ff" text-color="#fff" active-text-color="#ffd04b">
          <!-- 一级菜单 -->
          <el-submenu index="1">
            <!-- 一级菜单模板 -->
            <template slot="title">
              <!-- 图标 -->
              <i class="el-icon-location"></i>
              <!-- 文字 -->
              <span>导航一</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item index="1-4-1">
              <i class="el-icon-location"></i>
              <span>导航一</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧主体 -->
      <el-main>Main</el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  methods: {
    Logout () {
      window.sessionStorage.clear('token')
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
.el-header {
  background-color: #2cd5ff;
  display: flex;
  flex-direction: row;
  flex: row;
  align-items: center;
  justify-content: space-between;
  color: white;
  font-size: 20px;
}
.el-header > div {
  display: flex;
}
.el-aside {
  background-color: #2cd5ff;
}
.el-main {
  background-color: #ededed;
}
.home_container {
  height: 100%;
}
</style>
