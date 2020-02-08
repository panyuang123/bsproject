<template>
  <div class='login_container'>
    <div class="login_box">
      <!-- logo区域 -->
      <div class="avatar_box">
        <img src='../assets/logo.png'
             alt=''>
      </div>
      <!-- 登陆表单区域 -->
      <div>
        <el-form label-width="65px"
                 ref="loginFormRef"
                 :rules="loginFormRules"
                 :model="loginForm"
                 class="login_form">
          <el-form-item label="用户名"
                        prop='username'>
            <el-input prefix-icon="el-icon-user"
                      v-model="loginForm.username"></el-input>
          </el-form-item>
          <el-form-item label="密码"
                        prop='password'>
            <el-input prefix-icon="el-icon-lock"
                      v-model="loginForm.password"
                      type="password"></el-input>
          </el-form-item>
          <el-form-item class="btns">
            <el-button type="primary"
                       @click="login">登录</el-button>
            <el-button type="info"
                       @click='resetLoginForm'>重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const result = await this.$http.post('login', this.loginForm)
        console.log(result.data)
        if (result.data.meta.status !== 200) return this.$message.error('登陆失败')
        this.$message.success('登陆成功')
        window.sessionStorage.setItem('token', result.data.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style scoped>
.login_container {
  background-color: darkcyan;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: white;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.avatar_box {
  width: 130px;
  height: 130px;
  /* border: 1px solid #eee; */
  border-radius: 50%;
  background-color: white;
  padding: 10px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
}
.avatar_box img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
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
