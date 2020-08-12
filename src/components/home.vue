<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/logo.png" alt />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <!-- 侧边栏区域 -->
      <el-aside :width="isCollapse?'64px':'200px'">
        <div class="toggle-button" @click="toggle">|||</div>
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409eff"
          router
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          :default-active="activePath"
        >
          <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
            <template slot="title">
              <i :class="icon[item.id]"></i>
              <span>{{item.authName}}</span>
            </template>
            <el-menu-item
              :index="'/'+value.path"
              v-for="value in item.children"
              :key="value.id"
              @click="changePath('/'+value.path)"
            >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{value.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 主体部分 -->
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
      isCollapse: false,
      menuList: [],
      icon: {
        '125': 'iconfont icon-users',
        '103': 'iconfont icon-tijikongjian',
        '101': 'iconfont icon-shangpin',
        '102': 'iconfont icon-danju',
        '145': 'iconfont icon-baobiao',
      },
      activePath: '',
    }
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status != 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
    },
    toggle() {
      this.isCollapse = !this.isCollapse
    },
    changePath(val) {
      window.sessionStorage.setItem('activePath', val)
      this.activePath = val
    },
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
}
</script>
<style lang="less" scoped>
.toggle-button {
  background-color: #4a5064;
  cursor: pointer;
  text-align: center;
  font-size: 10px;
  line-height: 24px;
  color: #ffffff;
  letter-spacing: 0.2em;
}
.home-container {
  height: 100%;
}
.el-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 0;
  background-color: #373d41;
  color: #ffffff;
  font-size: 22px;
  div {
    display: flex;
    align-items: center;
    img {
      width: 50px;
      height: 50px;
      margin-right: 15px;
    }
  }
}
.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: none;
  }
  .iconfont {
    margin-right: 10px;
  }
}
.el-main {
  background-color: #eaedf1;
}
</style>
