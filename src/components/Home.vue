<template>
  <el-container class="home-container">
    <el-header>
      <div>
        <img src="../assets/logo.png" alt="">
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">logout</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width= "isCollapse? '64px' : '200px'" >
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu
              background-color="#545c64"
              text-color="#fff"
              active-text-color="#409bff"
              unique-opened
              :collapse="isCollapse"
              :collapse-transition="false"
              router
              :default-active="activePath">
              <!-- unique-opened 保证只有一个菜单打开collapse 是否折叠菜单collapse-transition折叠动画-->
              <!-- router使用vue-router模式，启用后会在激活导航时以index作为path进行路由跳转 -->
              <!-- 一级菜单 -->
              <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
                <!-- 一级菜单的模板区域 -->
                <template slot="title">
                  <!-- 图标 -->
                  <i class="el-icon-location"></i>
                  <!-- 文本 -->
                  <span>{{item.authName}}</span>
                </template>
                <!--二级菜单  -->
                <el-menu-item
                :index="'/' + subItem.path"
                v-for="subItem in item.children"
                :key="subItem.id"
                @click="saveNavState('/'+subItem.path)">
                  <template slot="title">
                    <!-- 图标 -->
                    <i class="el-icon-menu"></i>
                    <!-- 文本 -->
                    <span>{{subItem.authName}}</span>
                  </template>
                </el-menu-item>
              </el-submenu>
             </el-menu>
      </el-aside>
      <!-- 右侧内容主体 -->
      <el-main> <router-view></router-view> </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      // 左侧菜单数据
      menuList: [],
      isCollapse: false,
      // 被激活地址
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
    },
    // 点击按钮，切换菜单的折叠与展开
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 0;
  color: #fff;
  font-size: 20px;
  div{
    height: 100%;
    display: flex;
    align-items: center;
    img{
      width: 20%;
      height: 80%;
    }
    span{
      margin-left: 15px;
    }
  }
}
.el-aside {
  background-color: #333744;
  .el-menu{
    border-right: none;
  }
}
.toggle-button{
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  letter-spacing: 0.2em;
  text-align: center;
  cursor: pointer;
}
</style>
