<template>
    <el-form 
        :model="form" 
        ref="form"
        :rules="rules" 
        class="form">

        <el-form-item class="form-item" prop="username">
            <el-input 
            v-model="form.username"
            placeholder="用户名/手机">
            </el-input>
        </el-form-item>

        <el-form-item class="form-item" prop="password">
            <el-input 
            v-model="form.password"
            placeholder="密码" 
            type="password">
            </el-input>
        </el-form-item>

        <p class="form-text">
            <nuxt-link to="#">忘记密码</nuxt-link>
        </p>
     
        <el-button 
        class="submit"
        type="primary"
        @click="handleLoginSubmit"
        >
            登录
        </el-button>
           <!--
        这里要添加@keyup.enter="handleLoginSubmit"按下键盘enter登录，由于
        这里的el-button是一个组件，如果这个组件不触发按下键盘enter登录这个事件，
        也就是没有办法实现登录。如果要实现就只能把el-button换成button，就可以。
        -->
        <!--
        <button 
        class="submit"
        type="primary"
        @click="handleLoginSubmit"
        @keyup.enter="handleLoginSubmit"
        >
            登录
        </button>
        -->

    </el-form>

</template>

<script>
export default {
    data(){
        return {
            // 表单数据
            form: {
                username:"",
                password:""
            },
            // 表单规则
            rules: {
                username:[
                    // required 必填  message 错误信息  trigger 失去焦点时触发
                    {required:true,message:'请输入用户名',trigger:'blur'},
                ],
                password:[
                    {required:true,message:'请输入用户密码',trigger:'blur'},
                ]
            },
        }
    },
    //vuex不能通过直接赋值方式来修改state的值 
    methods: {
        // 提交登录
         handleLoginSubmit(){
        //    console.log(this.form)
            this.$refs.form.validate(async valid => {
                // valid是表单验证的结果
                // console.log(valid)
                if(valid){
                    //提交登录接口
                   const res = await this.$store.dispatch("user/login",this.form)
                            if(res.status === 200){
                                this.$message.success("登录成功");

                                this.$router.push('/')
                            }

                        }
                
                })
             }
        }
}
</script>

<style scoped lang="less">
    .form{
        padding:25px;
    }

    .form-item{
        margin-bottom:20px;
    }

    .form-text{
        font-size:12px;
        color:#409EFF;
        text-align: right;
        line-height: 1;
    }

    .submit{
        width:100%;
        margin-top:10px;
    }
</style>
