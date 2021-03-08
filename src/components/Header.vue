<template>

<!--  <div class="m-content">

&lt;!&ndash;标题及个人信息
&ndash;&gt;
      <div style="float: left">
        <el-link href="/">
          <img src="../assets/logo2.png" style="height: 50px;">
        </el-link>
      </div>

    <div class="m-user-content">
      <div class="m-right" v-if="hasLogin">
        <el-divider direction="vertical"></el-divider>
        <span><el-link type="danger" @click="logout">退出</el-link></span>
      </div>
      <div class="m-right" v-if="!hasLogin">
        <el-divider direction="vertical"></el-divider>
        <span><el-link type="danger" @click="login" href="/login">登录</el-link></span>
      </div>

      <div class="m-right">

        <span>
            <el-col :span="30">
            <el-dropdown>
              <span class="el-dropdown-link">
                个人中心<i class="el-icon-arrow-down el-icon&#45;&#45;right"></i>
              </span>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item icon="el-icon-plus"><el-link type="success">个人信息</el-link></el-dropdown-item>
                <el-dropdown-item icon="el-icon-circle-plus"><el-link type="success">捐献记录</el-link></el-dropdown-item>
                <el-dropdown-item icon="el-icon-circle-plus"><el-link type="success">我的订单</el-link></el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </el-col>
        </span>
      </div>

      <div class="m-right">

        <span class="m-user-login">{{user.username}}</span>
        <el-divider direction="vertical"></el-divider>
      </div>
      <div class="m-right">

        <span class="m-user-login"><el-link type="danger" href="/blog/add">发表</el-link></span>
        <el-divider direction="vertical"></el-divider>
      </div>
    </div>

    <el-avatar :size="50" :src="user.avatar" style="float: right"></el-avatar>


  </div>-->
  <div style="height: 80px ;width: 960px;margin: 0 auto;">

      <el-link href="/">
        <img src="../assets/logo2.png" style="height: 50px;width: 138px">
      </el-link>

    <div style="width: 600px;float:right;">
      <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect" style="text-align: center;margin: 0 auto;">


        <el-menu-item index="1">{{user.username}}</el-menu-item>
        <el-menu-item index="2"><el-link type="primary" href="/index">首页</el-link></el-menu-item>
        <el-menu-item index="3"><el-link type="primary" href="/">我要挂售</el-link></el-menu-item>
        <el-menu-item index="4"><el-link type="primary" href="/blogs">论坛中心</el-link></el-menu-item>
        <el-submenu index="5">
          <template slot="title">个人中心</template>
          <el-menu-item index="5-1"><el-link type="primary" href="/">我的信息</el-link></el-menu-item>
          <el-menu-item index="5-2"><el-link type="primary" href="/">我的订单</el-link></el-menu-item>
        </el-submenu>
        <!--<el-submenu index="2">
          <template slot="title">我的工作台</template>
          <el-menu-item index="2-1">选项1</el-menu-item>
          <el-menu-item index="2-2">选项2</el-menu-item>
          <el-menu-item index="2-3">选项3</el-menu-item>
          <el-submenu index="2-4">
            <template slot="title">选项4</template>
            <el-menu-item index="2-4-1">选项1</el-menu-item>
            <el-menu-item index="2-4-2">选项2</el-menu-item>
            <el-menu-item index="2-4-3">选项3</el-menu-item>
          </el-submenu>
        </el-submenu>
        <el-menu-item index="3" disabled>消息中心</el-menu-item>
        <el-menu-item index="4"><a href="https://www.ele.me" target="_blank">订单管理</a></el-menu-item>-->
        <el-menu-item index="6">
          <span v-show="!hasLogin"><el-link type="primary" href="/login">登录</el-link></span>
          <span v-show="hasLogin"><el-link type="primary" @click="logout">退出</el-link></span>
        </el-menu-item>
        <el-avatar :size="50" :src="user.avatar" style="float: right"></el-avatar>
      </el-menu>

    </div>
  </div>

</template>

<script>
export default {
  name: "Header",
  data(){
    return{
      user:{
        username:'请先登录',
        avatar:'https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png'
      },
      hasLogin: false
    }
  },
  methods: {
    logout() {
      const _this = this
      this.$axios.get('http://localhost:8880/logout', {
        headers: {
          "Authorization": localStorage.getItem("token")
        }
      }).then((res) => {
        _this.$store.commit('REMOVE_INFO')
        _this.$router.push('/login')
      });
    },
    
  },
  created() {
    if(this.$store.getters.getUser.username) {
      this.user.username = this.$store.getters.getUser.username
      this.user.avatar = this.$store.getters.getUser.avatar
      this.hasLogin = true
    }
  }


}
</script>

<style scoped>
.m-content{
  height: 60px;
}
.m-user-content{
  float: right;
  margin: 20px;
}
.m-right{
  float: right;
}
.m-user-login{
  font-size: 15px;
}

</style>