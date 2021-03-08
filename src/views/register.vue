<template>
    <el-container>
        <el-header>
            <img src="../assets/logo2.png" style="height: 80%;margin-top: 10px">
        </el-header>
        <el-main>
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                <el-form-item label="账号:" prop="username">
                    <el-input v-model="ruleForm.username"></el-input>
                </el-form-item>
                <el-form-item label="密码:" prop="password">
                    <el-input type="password" v-model="ruleForm.password"></el-input>
                </el-form-item>

                <el-form-item label="邮箱" prop="email">
                    <el-input v-model="ruleForm.email"></el-input>
                </el-form-item>

                <el-form-item>
                    <el-button type="primary" @click="submitForm('ruleForm')">注册</el-button>
                    <el-button @click="gotoLogin()">返回登录</el-button>
                </el-form-item>
            </el-form>
        </el-main>

    </el-container>
</template>

<script>
    import Header from "../components/Header";
    export default {
        name: "register",
        components: {
            Header
        },
        data(){
            return{
                ruleForm: {
                    username : '',
                    password : '',
                    email: ''
                },
                rules: {
                    username: [
                        { required: true, message: '请输入账号', trigger: 'blur' },
                        { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'change' }
                    ],

                    email: [
                        { required: true, message: '请输入邮箱', trigger: 'change' }
                    ]
                }
            };

        },

        methods:{
            submitForm(formName){
                const _this = this
                this.$refs[formName].validate((valid) => {

                    if(valid){
                        this.$axios.get('http://localhost:8880/user/edit',this.ruleForm).then((res) =>{
                            _this.$alert('注册成功','提示',{
                                confirmButtonText:'确定',
                                callback:action => {
                                    _this.$router.push("/login")
                                }
                            });

                        });
                    }else{
                        return false;
                    }
                })
            },
            gotoLogin(){
                this.$router.push("/login");
            }
        }

    }
</script>

<style scoped>
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