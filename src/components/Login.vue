<template>
    <div class="login_container">
    <div class="login_box">
<div class='avatar_box'>
    <img src="../assets/logo.png" alt="">
</div>
<!--表单 -->
<el-form  ref="loginReset"  :model="form" :rules="LoginRules" label-width="0px" class="login_form">
  <el-form-item  prop="username">
    <el-input  v-model="form.username" prefix-icon="iconfont icon-user"></el-input>
  </el-form-item>

   <el-form-item prop="password">
    <el-input  type="password" v-model="form.password"  prefix-icon="iconfont icon-3702mima"></el-input>
  </el-form-item>

  <el-form-item class='btns'>
  <el-button type="primary" @click="submitForm">提交</el-button>

  <el-button type="info" @click="resetFrom">重置</el-button>
  </el-form-item >

  </el-form>
    </div>

    </div>
</template>
<script>
export default {
  data () {
    return {
      form: {
        username: 'admin',
        password: '123456'
      },
      LoginRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 15 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetFrom () {
      this.$refs.loginReset.resetFields()
    },
    submitForm () {
      this.$refs.loginReset.validate(async valid => {
        console.log(valid)
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.form)
        console.log(res)
        if (res.meta.status === 200) return this.$message('登录失败')
        // this.$http.post('login', this.form)
        //   .then(res => {
        //     console.log(res.data)
        //   })
        //   .catch(err => {
        //     alert(err)
        //   })
        // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
        //   1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
        //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
        window.sessionStorage.setItem('token', res.meta)
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push('/home')
      }
      )
    }
  }
}
</script>
<style lang="less" scoped>
.login_container{
    background-color:#2b4b6b;
    height: 100%;
}
.login_box{
    width:450px;
    height:300px;
    position: absolute;
    left:50%;
    top:50%;
    background-color: #fff;
   transform: translate(-50%,-50%);

}
.avatar_box
{
height: 130px;
width:130px;
border:1px solid #eee;
border-radius: 50%;
position:absolute;
left:50%;
transform:translate(-50%,-50%);
padding:10px;
box-shadow: 0 0 10px #ddd;
background-color:#fff;

img{
    width:100%;
    height:100%;
    border-radius: 50%;
    background-color: #eee;
    border-radius: 50%;
}
}
.login_form{
    position:absolute;
    bottom:0;
    width:100%;
    padding: 0 20px;
    box-sizing:border-box;
}

</style>
