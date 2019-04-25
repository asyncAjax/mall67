<template>
  <el-container>
    <el-header>
      <div class="logo-box">
        <img src="../assets/img/heima.png" alt>
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout()">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isshow?'65px':'200px'">
        <div class="toggle_bar" @click="isshow=!isshow" :style="{width:isshow?'65px':'200px'}">|||</div>
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409EFF"
          :unique-opened="true"
          :collapse-transition="false"
          :collapse="isshow"
          :router="true"
        >
          <el-submenu
            :index="item.id"
            :style="{width:isshow?'65px':'200px'}"
            v-for="(item,k) in menuList"
            :key="item.id"
          >
            <template slot="title">
              <i :class="'iconfont icon-'+iconList[k]"></i>
              <span>{{ item.authName }}</span>
            </template>
            <el-menu-item
              v-for="item2 in item.children"
              :key="item2.id"
              :index="item2.path"
            >
              <i class="el-icon-menu"></i>
              <span>{{ item2.authName }}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view/>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  // 生命周期函数
  created() {
    this.getMenuList()
  },
  methods: {
    // 管理员退出系统
    logout() {
      this.$confirm('确认要退出系统么?', '退出', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          // 删除token
          window.sessionStorage.removeItem('token')
          // 重定向到登录页面
          this.$router.push('/login')
        })
        .catch(() => {
        })
    },
    // (axios)获得左侧导航数据信息
    async getMenuList() {
      const { data: dt } = await this.$http.get('/menus')
      console.log(dt)
      if (dt.meta.status !== 200) {
        return this.$message.error(dt.meta.msg)
      }

      // 把获得到导航权限数据 赋予给menuList成员
      this.menuList = dt.data
    }
  },
  data() {
    return {
      // 图标系列
      iconList: ['users', 'tijikongjian', 'shangpin', 'danju', 'baobiao'],
      // 接受左侧菜单的数据成员
      menuList: [],
      // 左侧导航菜单折叠控制开关
      isshow: false
    }
  }
}
</script>

<style lang="less" scoped>
// .el-submenu__title{
//   font-size:12px;
// }
.el-container {
  height: 100%;
  .el-header {
    background-color: #373d41;
    display: flex;
    align-items: center;
    padding: 0;
    padding-right: 20px;
    justify-content: space-between;
    .logo-box {
      color: #fff;
      font-size: 22px;
      display: flex;
      align-items: center;
      user-select: none;
      img {
        width: 50px;
        height: 50px;
        margin-right: 10px;
      }
    }
  }
  .el-aside {
    background-color: #333744;
    /*div收起展开样式设置*/
    .toggle_bar {
      width: 200px;
      height: 25px;
      background-color: #4a5064;
      color: #fff;
      text-align: center;
      font-size: 12px;
      line-height: 25px;
      letter-spacing: 0.1em;
      user-select: none;
      cursor: pointer;
    }
  }
  .el-main {
    background-color: #eaedf1;
  }
}
</style>
