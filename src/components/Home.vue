<template>
  <el-container>
    <el-header>
      <div id="logo-box">
        <img
          src="../assets/img/heima.png"
          alt=""
        >
        <span>电商后台管理系统</span>
      </div>
      <el-button
        type="info"
        @click="logout"
      >退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="menushow ? '65px' : '200px'">
        <div
          style="height:25px;color:white;text-align:center;line-height:25px;
        background-color:#4a5064;font-size:12px;letter-spacing:0.1em;
        cursor:pointer;user-select:none"
          @click="menushow=!menushow"
        >|||</div>
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409eff"
          :collapse="menushow"
          :collapse-transition="false"
          :unique-opened="true"
          :router="true"
        >
          <el-submenu
            :index="item.id+''"
            v-for="(item,k) in menuList"
            :key="item.id"
            :style="menushow?'width:65px;':'width:200px;'"
          >
            <template slot="title">
              <i :class="'iconfont icon-' + menuicon[k]"></i>
              <span>{{item.authName}}</span>
            </template>
            <el-menu-item
              :index="item2.path"
              v-for="item2 in item.children"
              :key="item2.id"
            >
              <i class="el-icon-menu"></i>
              <span>{{item2.authName}}</span>
            </el-menu-item>

          </el-submenu>

        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  created() {
    this.getMenuList()
  },
  data() {
    return {
      menushow: false,
      menuList: [],
      menuicon: ['users', 'tijikongjian', 'shangpin', 'danju', 'baobiao']
    }
  },
  methods: {
    // 获得左侧功能按钮数据
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) {
        return this.$message.error(res.meta.msg)
      }
      // console.log(res)
      this.menuList = res.data
    },

    logout() {
      this.$confirm('Sure?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          window.sessionStorage.removeItem('token')
          this.$router.push('/login')
        })
        .catch(() => {})
    }
  },
  components: {}
}
</script>

<style scoped lang="less">
.el-container {
  height: 100%;
  .el-header {
    background-color: #373d41;
    padding: 0;
    padding-right: 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    #logo-box {
      font-size: 22px;
      color: #ffffff;
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
  }
  .el-main {
    background-color: #eaedf1;
  }
}
</style>
