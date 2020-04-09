<template>
  <div class="login_page">
    <div class="login_box">
      <div class="login_img">
        <img src="../assets/logo.png" alt />
      </div>
      <el-form
        :model="LoginForm"
        :rules="rules"
        ref="loginform"
        label-width="0px"
        class="login_form"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input prefix-icon="iconfont iconfolder" v-model="LoginForm.username"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            type="password"
            prefix-icon="iconfont iconpassword"
            v-model="LoginForm.password"
          ></el-input>
        </el-form-item>
        <el-form-item class="login_btn">
          <el-button type="primary" @click="Login">登录</el-button>
          <el-button type="info" @click="Reset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      LoginForm: {
        username: 'admin',
        password: '123456'
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    Reset() {
      console.log(this)
      this.$refs.loginform.resetFields()
    },
    Login() {
      this.$refs.loginform.validate(async valid => {
        console.log(valid)
        if (!valid) return
        // // eslint-disable-next-line no-unused-vars
        const { data: res } = await this.$http.post('login', this.LoginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功！')
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style scoped>
.login_page {
  background-color: #11c4f1;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  border-radius: 4px;
  background-color: white;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.login_img {
  height: 130px;
  width: 130px;
  padding: 15px;
  box-shadow: 0 0 10px #dddd;
  border-radius: 50%;
  position: absolute;
  left: 50%;
  top: -50%;
  transform: translate(-50%, 50%);
  background-color: white;
}
img {
  border-radius: 50%;
  height: 100%;
  width: 100%;
  background-color: #797979;
}

.login_form {
  position: absolute;
  width: 100%;
  padding: 0 20px;
  bottom: 0;
  box-sizing: border-box;
}
.login_btn {
  display: flex;
  justify-content: flex-end;
}
</style>
