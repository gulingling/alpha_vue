<template>
  <el-form class="login-container" label-position="left"
           label-width="0px">
    <h3 class="login_title">系统登录</h3>
    <el-form-item>
      <el-input type="text" v-model="loginForm.username"
                auto-complete="off" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item>
      <el-input type="password" v-model="loginForm.password"
                auto-complete="off" placeholder="密码"></el-input>
    </el-form-item>
    <el-form-item style="width: 100%">
      <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="login">登录</el-button>
      <router-link to="registerEnterprise"><el-button type="primary" style="width: 40%;background: #505458;border: none">注册</el-button></router-link>
    </el-form-item>
  </el-form>
</template>


<script>

  export default {
    name: 'Login',
    data () {
      return {
        loginForm: {
          username: '',
          password: ''
        },
        responseResult: []
      }
    },
    methods: {     
      login () {
        var _this = this
        console.log(this.$store.state)
        this.$axios
          .post('/login', {           
            username: this.loginForm.username,
            password: this.loginForm.password
          })
          .then(resp => {
            if (resp.data.code === 200) {
              // var data = this.loginForm
              _this.$store.commit('login', _this.loginForm)
              var path = this.$route.query.redirect
              this.$router.replace({path: path === '/' || path === undefined ? '/admin' : path})
            } else {
              this.$alert(resp.data.message, '提示', {
                confirmButtonText: '确定'
              })
            }
          })
          .catch(failResponse => {
          })
      },
      keyDown(e){ //如果是回车则执行登录方法
        if (e.keyCode == 13) {
          this.login();
        }
      }
    },
    mounted () {
      //绑定事件
     window.addEventListener('keydown',this.keyDown);
    },
    destroyed () {
      window.removeEventListener('keydown',this.keyDown,false);
    }
  }
</script>
<style  scoped>
.login-container {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 90px auto;
  width: 350px;
  padding: 35px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}
  
.login_title {
  margin: 0px auto 40px auto;
  text-align: center;
  color: #505458;
}


</style>
