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

                <el-form-item label="确认密码" prop="password1">
                    <el-input type="password" v-model="ruleForm.password1"></el-input>
                </el-form-item>

                <el-form-item label="邮箱" prop="email">
                    <el-input v-model="ruleForm.email"></el-input>
                </el-form-item>

                <el-form-item label="地址" prop="address">
                    <el-input v-model="ruleForm.address"></el-input>
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
                    password1: '',
                    email: '',
                    address: ''
                },
                rules: {
                    username: [
                        { required: true, message: '请输入账号', trigger: 'blur' },
                        { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'change' }
                    ],
                    password1: [
                        { required: true, message: '请再次输入密码', trigger: 'change' }
                    ],
                    email: [
                        { required: true, message: '请输入邮箱', trigger: 'change' }
                    ]
                }
            };

        },

        methods{
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

</style>