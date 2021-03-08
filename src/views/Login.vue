<template>
  <div>
    <el-container>
      <el-header>
        <img src="../assets/logo2.png" style="height: 80%;margin-top: 10px">
      </el-header>
      <el-main>
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">

          <el-form-item label="帐号:" prop="username">
            <el-input  v-model="ruleForm.username"></el-input>
          </el-form-item>

          <el-form-item label="密码:" prop="password">
            <el-input type="password" v-model="ruleForm.password"></el-input>
          </el-form-item>


          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
            <el-button @click="gotoRegister()">注册</el-button>
          </el-form-item>
        </el-form>
      </el-main>

    </el-container>
  </div>
</template>
<script>

import axios from "axios";
export default {
  name:"login",
  data() {
    return {
      ruleForm: {
        username: '',
        password: '',
      },
      rules: {
        username: [
          { required: true, message: '请输入账号', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'change' }
        ]
      },
      /*footer*/
      footer_show: true
    };
  },

  methods: {
    submitForm(formName) {
      const _this = this
      this.$refs[formName].validate((valid) => {
        if (valid) {
          // 提交逻辑
          this.$axios.post('http://localhost:8880/login', this.ruleForm).then((res)=>{
            const token = res.headers['authorization']
            _this.$store.commit('SET_TOKEN', token)
            _this.$store.commit('SET_USERINFO', res.data.data)
            /*跳转*/
            _this.$router.push("/index")
            console.log(res.data.data)
          })
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },

    gotoRegister(){
      this.$router.push("/register");
    },

    refreshCode() {
      this.identifyCode = "";
      this.makeCode(this.identifyCodes, 4);
    },

    resetForm(formName) {
      this.$refs[formName].resetFields();
    },


  }
}
</script>
<style>
.el-header, .el-footer {
  /*background-color: #B3C0D1;*/
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-aside {
  background-color: #D3DCE6;
  color: #333;
  text-align: center;
  line-height: 200px;
}

.el-main {
  background-color: #E9EEF3;
  color: #333;
  text-align: center;
  line-height: 160px;
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}

/*111111111111111111111111111111111111*/
.demo-ruleForm{
  max-width: 500px;
  margin: 0 auto;
}
.validate-code {
  width: 136px;
  float: left;
}
.code {
  width: 112px;
  height: 35px;
  border: 1px solid #ccc;
  float: right;
  border-radius: 2px;
}
</style>