<template>
  <div class="login_body">
    <div class="bg" />
    <div class="box">
      <div class="title">智慧校园-登录</div>
      <!-- 表单校验 四要素 -->
      <!-- el-form :model 对应的值是表单对应的数据对象 -->
      <!-- el-form :rules 对应的校验规则 -->
      <!-- el-form-item  prop 表示要检验哪个字段  -->
      <!-- 表单元素(el-input ,el-checkbox ...)  v-model-->
      <el-form ref="form" :model="loginForm" :rules="rules">
        <el-form-item label="账号" prop="username">
          <el-input  v-model="loginForm.username"/>
        </el-form-item>

        <el-form-item label="密码" prop="password">
          <el-input v-model="loginForm.password" show-password/>
        </el-form-item>

        <el-form-item prop="remember">
          <el-checkbox v-model="rememberMe">记住我</el-checkbox>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" class="login_btn"
          @click="handleLogin"
          >登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import{FORM_KEY} from '@/constants/KEY'
export default {
  name: 'Login',
  data(){
    return{
      loginForm:{
        username:'',
        password:''
      },
      rememberMe:true,
      rules:{
        username:[{required:true,message:'用户名不可为空',trigger:'blur'}],
        password:[{required:true,message:'密码不可为空',trigger:'blur'}]
      }
    }
  },
  created(){
    const loginData = localStorage.getItem(FORM_KEY)
    // 先判断有没有存储用户信息
    if(loginData){
      const {username,password} = JSON.parse(loginData)
      this.loginForm.username = username
      this.loginForm.password = password
    }
  },
  methods:{
    handleLogin(){
      // this.$refs['form']
      this.$refs.form.validate(async(flag)=>{
        if(!flag) return 
        // const res = await loginAPI(this.loginForm)
        // this.$store.commit('user/setToken',res.data.token)
        await this.$store.dispatch('user/loginAction',this.loginForm)
          // 判断用户是否点击了 记住我
          if(this.rememberMe){
            localStorage.setItem(FORM_KEY,JSON.stringify(this.loginForm))
          }else{
            localStorage.removeItem(FORM_KEY)
          }
          // 实现那个页面退出的，登录时继续跳转到对应的页面
          if(this.$route.query.required){
            this.$router.push(this.$route.query.required)
          }else{
            this.$router.push('/')
          }
       
      })
    }
  }
}

</script>

<style scoped lang="scss">
.login_body {
  display: flex;
}

.bg {
  width: 60vw;
  height: 100vh;
  background: url('~@/assets/login-bg.svg') no-repeat;
  background-position: right top;
  background-size: cover;
}

.box {
  margin: 200px 10% 0;
  flex: 1;

  .title {
    padding-bottom: 76px;
    font-size: 26px;
    font-weight: 500;
    color: #1e2023;
  }

  ::v-deep() {
    .ant-form-item {
      display: flex;
      margin-bottom: 62px;
      flex-direction: column;
    }

    .ant-form-item-label label {
      font-size: 16px;
      color: #8b929d;
    }

    .ant-input,
    .ant-input-password {
      border-radius: 8px;
    }
  }
}

.login_btn {
  width: 100%;
}
</style>
