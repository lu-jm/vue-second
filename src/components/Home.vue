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
      <el-aside :width="isCollapse ? '65px' : '200px'">
        <div class="toggle-button" @click="toggleCillapse">|||</div>
        <!-- 左侧菜单 -->
        <el-menu
          background-color="#2cd5ff"
          text-color="#fff"
          active-text-color="#ffd04b"
          :unique-opened="true"
          :collapse="isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <el-submenu v-for="item in menulist" :key="item.id" :index="item.id+' '">
            <!-- 一级菜单模板 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文字 -->
              <span>{{item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item
              v-for="subitem in item.children"
              :key="subitem.id"
              :index="'/'+subitem.path"
              @click="saveNavstatus('/'+subitem.path)"
            >
              <i class="el-icon-menu"></i>
              <span>{{subitem.authName}}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧主体 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menulist: [],
      iconsObj: {
        125: 'iconfont iconbussiness-man-fill',
        103: 'iconfont iconproduct',
        101: 'iconfont iconNewuserzone-fill',
        102: 'iconfont iconorder-fill',
        145: 'iconfont iconbrowse'
      },
      isCollapse: false,
      // 被激活的链接地址
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activepath')
  },
  methods: {
    Logout() {
      window.sessionStorage.clear('token')
      this.$router.push('/login')
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      console.log(res)
    },
    toggleCillapse() {
      this.isCollapse = !this.isCollapse
    },
    // 保存链接激活状态
    saveNavstatus(activepath) {
      window.sessionStorage.setItem('activepath', activepath)
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
.el-menu {
  border-right: 0px;
}
.el-main {
  background-color: #ededed;
}
.home_container {
  height: 100%;
}
.iconfont {
  margin-right: 10px;
}
.toggle-button {
  background-color: #4a5064;
  font-size: 10px;
  text-align: center;
  line-height: 24px;
  color: white;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
