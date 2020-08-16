<!-- 登录组件 -->
<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像 -->
      <div class="avant_box">
        <img src="../assets/logo.png" alt="avant">
      </div>
      <el-form ref="form" :model="form" label-width="0px" class="login_form" :rules="rules">
        <el-form-item prop="username">
          <el-input v-model="form.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="form.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="reset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      // 登录表单数据对象
      form: {
        username: '',
        password: ''
      },
      // 表单验证规则对象
      rules: {
        // 用户名是否合法
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 密码是否合法
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },

  methods: {
    reset () {
      this.$refs.form.resetFields()
    },
    login () {
      this.$refs.form.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.form)
        if (res.meta.status !== 200) return this.$message.error('登陆失败，用户名或密码错误')
        this.$message.success('登陆成功')
        // 保存token到sessionStorage中
        window.sessionStorage.setItem('token', res.data.token)
        // 跳转到home页面
        this.$router.push('/home')
      })
    }
  }

}

</script>

<style lang='less' scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
  .login_box {
    position: absolute;
    background-color: #fff;
    width: 450px;
    height: 300px;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    border-radius: 3px;
    .avant_box {
      height: 130px;
      width: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      background-color: #fff;
      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
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
