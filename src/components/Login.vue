<template>
  <div id="login-container">
    <div id="login-box">
      <div id="login-logo">
        <img
          src="../assets/img/logo.png"
          alt
        >
      </div>
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="loginFormRules"
      >
        <el-form-item prop="username">
          <el-input v-model="loginForm.username">
            <i
              slot="prefix"
              class="iconfont icon-user"
            ></i>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            type="password"
            v-model="loginForm.password"
          >
            <i
              slot="prefix"
              class="iconfont icon-3702mima"
            ></i>
          </el-input>
        </el-form-item>

        <el-row>
          <el-col :offset="15">
            <el-button
              type="primary"
              @click="login"
            >登陆</el-button>
            <el-button
              type="info"
              @click="resetForm"
            >重置</el-button>
          </el-col>
        </el-row>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' }
        ],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    login() {
      // 对form表单进行校验
      this.$refs.loginFormRef.validate(async valid => {
        // valid:true 校验成功
        // valid:false 校验成功

        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) {
          return this.$message.error('用户名或密码不正确')
        }
        this.$message.success('登录成功')

        window.sessionStorage.setItem('token', res.data.token)
        // console.log(res)
        this.$router.push('/home')
      })
    },
    resetForm() {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>

<style scoped lang="less">
#login-container {
  background-color: #2b4b6b;
  height: 100%;
  #login-box {
    width: 450px;
    height: 304px;
    border: 1px #fff solid;
    background-color: #fff;
    border-radius: 4px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    #login-logo {
      width: 130px;
      height: 130px;
      background-color: #fff;
      // border:1px #ccc solid;
      box-shadow: 0px 0px 20px #eee;
      border-radius: 50%;
      padding: 8px;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eeeeee;
      }
    }
    .el-form {
      width: 100%;
      position: absolute;
      bottom: 0;
      padding: 20px;
      box-sizing: border-box;
    }
  }
}
</style>
