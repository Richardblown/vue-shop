<template>
  <el-container class="container">
    <el-header>
      <div>
        <img src='../assets/manage.png' class="head-img">
        <span>后台管理系统</span>
      </div>
      <el-button type='info' @click='logout'>退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapse ? '64px':'200px'">
        <div class="toggle-button" @click='toggle'>|||</div>
          <el-menu
            background-color="#333744"
            text-color="#fff"
            active-text-color="#409EFF"
            unique-opened
            :collapse='isCollapse'
            :collapse-transition='false'
            :router='true'
            default-active='/users'>
            <!--一级菜单-->
            <el-submenu :index="'/'+ i.path " v-for='i in list' :key="i.id">
              <template slot="title">
                <i :class="icons[i.id]"></i>
                <span>{{i.authName}}</span>
              </template>
              <!--二级菜单-->
              <el-menu-item :index="'/'+ j.path " v-for='j in i.children' :key="j.id" @click='saveStatus("/"+ j.path)'>
                <template slot="title">
                  <i class="el-icon-menu"></i>
                  <span>{{j.authName}}</span>
                </template>
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
  data() {
    return {
      list: [],
      icons: {
        125: 'iconfont iconyonghuming',
        103: 'iconfont iconguanli',
        101: 'iconfont iconguanlizhongxin',
        102: 'iconfont iconguanliwenzhang',
        145: 'iconfont icontongji'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    // 登录退出
    logout (){
      window.sessionStorage.clear()
      this.$router.push('login')
    },
    // 获取左侧菜单栏
    async getMenuList(){
      const { data: res } = await this.$http.get('menus')
      if(res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.list = res.data
    },
    // 左侧菜单折叠
    toggle(){
      this.isCollapse = !this.isCollapse
    },
    // 保存当前地址
    saveStatus(path){
      window.sessionStorage.setItem('activePath', path)
      this.activePath = path
    }
  }
}
</script>

<style lang="less" scoped>
.container{
  height: 100%;
}
.head-img{
  height: 30px;
  width: 30px;
}
.el-header{
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  >div{
    display: flex;
    align-items: center;
    span{
      margin-left: 15px;
    }
  }
}
.el-aside{
  background-color: #333744;
  .el-menu{
    border-right: 0;
  }
}
.el-main{
  background-color: #EAEDF1;
}
.iconfont{
  margin-right: 20px;
}
.toggle-button{
  background-color: 4A5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 2px;
  cursor: pointer;
}
</style>
