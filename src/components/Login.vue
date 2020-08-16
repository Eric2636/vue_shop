<template>
    <div class="login_container">
        <div class="login_box">

            <!-- 头像区域 -->
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="">

                

            </div>

            <!-- 登录表单区域 -->
                <el-form ref="resetLoginForm" :model="loginForm" :rules="LoginFormRules" label-width="0px" class="login_form">
                    <!-- 用户名 -->
                        <el-form-item  prop="username">
                            <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
                        </el-form-item> 
                    <!-- 密码 -->
                        <el-form-item  prop="password"> 
                            <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
                        </el-form-item> 
                    <!-- 按钮区域 -->
                    <el-form-item  class="btns">
                            <el-button @click="login" type="primary">提交</el-button>
                            <el-button @click="resetForm" type="info">重置</el-button>

                    </el-form-item>
                </el-form> 

        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            loginForm: {
               username:'admin',
               password:'123456' 
            },
            // 表单验证的规则
            LoginFormRules:{
                username:[
                    //注意：v-model里填写的属性要和prop的属性保持一致，不然只能验证一行规则
                    { required: true, message: '请输入用户名', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
                ],
                
                password:[
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
                ]
            }

        }
    },
    methods:{
        resetForm(){
            this.$refs.resetLoginForm.resetFields()
        },

        login(){
            this.$refs.resetLoginForm.validate(async(valid)=>{
                if(!valid) return
                  const {data} = await this.$axios.post("login",this.loginForm) 

                  //console.log(data)
                    if(data.meta.status == 200){
                        this.$message.success('登录成功');
                        
                        window.sessionStorage.setItem("token",data.data.token)
                        this.$router.push("/home")
                    }else{
                        this.$message.error('登录失败');

                    }
            })
        }
    }
}
</script>

<style lang="less" scope>
    .login_container {
        background-color: #2b4b6b;
        height: 100%;
    }

    .login_box {
        width: 450px;
        height: 300px;
        background-color: #fff;
        border-radius: 3px;
        position: absolute;
        left:50%;
        top: 50%;
        transform: translate(-50%,-50%);
        
    
        
    }

    .avatar_box{
        width: 130px;
        height: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%,-50%);
        background-color: #fff;
        img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
        }
    }


    .login_form {
        position: absolute;
        bottom: 0;
        width: 100%;
        padding: 0 20px;
        box-sizing: border-box;

    }


    .btns {
        display: flex;
        justify-content: flex-end;
    }


</style>