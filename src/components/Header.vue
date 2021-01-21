<template>

  <div class="m-content">

<!--标题及个人信息
-->
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
                个人中心<i class="el-icon-arrow-down el-icon--right"></i>
              </span>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item icon="el-icon-plus"><el-link type="success">个人信息</el-link></el-dropdown-item>
                <el-dropdown-item icon="el-icon-circle-plus"><el-link type="success">捐献记录</el-link></el-dropdown-item>
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
      }
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
    }
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