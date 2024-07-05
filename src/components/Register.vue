<template>
  <div class="login_page">
    <transition name="form-fade" mode="in-out">
      <section class="form_container" v-show="showLogin">
        <div class="manage_tip">
          <p>后台管理系统</p>
        </div>
        <el-form :model="registerForm" :rules="rules" ref="loginForm">
          <el-form-item prop="username">
            <el-input v-model="registerForm.username" placeholder="用户名"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input type="password" placeholder="密码" v-model="registerForm.password"></el-input>
          </el-form-item>
          <el-form-item prop="email">
            <el-row>
              <el-col :span="16"> <!-- 控制输入框宽度 -->
                <el-input v-model="registerForm.email" placeholder="邮箱"></el-input>
              </el-col>
              <el-col :span="4"> <!-- 控制按钮宽度 -->
                <el-button @click="getVerificationCode()" :disabled="!registerForm.email">获取验证码</el-button>
              </el-col>
            </el-row>
          </el-form-item>
          <el-form-item prop="verificationCode">
            <el-input v-model="registerForm.verificationCode" placeholder="验证码"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="register()" class="submit_btn">注册</el-button>
          </el-form-item>
        </el-form>
      </section>
    </transition>
  </div>
</template>

<style lang="less" scoped>
@import '../style/mixin';

html, body {
  height: 100%;
  margin: 0;
  padding: 0;
}

.login_page {
  background-color: #324057;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中 */
}

.manage_tip {
  position: absolute;
  width: 100%;
  top: -100px;
  left: 0;

  p {
    font-size: 34px;
    color: #fff;
  }
}

.form_container {
  width: 320px;
  height: 280px;
  padding: 25px;
  border-radius: 5px;
  text-align: center;
  background-color: #fff;
  position: relative; /* 确保定位不影响布局 */

  .submit_btn {
    width: 100%;
    font-size: 16px;
  }
}

.form-fade-enter-active, .form-fade-leave-active {
  transition: all 1s;
}

.form-fade-enter, .form-fade-leave-active {
  transform: translate3d(0, -50px, 0);
  opacity: 0;
}

</style>

<script>
export default {
  data() {
    return {
      registerForm: {
        username: '',
        password: '',
        email: '',
        verificationCode: '',
      },
      rules: {
        username: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'}
        ],
        email: [
          { required: true, message: '请输入邮箱', trigger: 'blur' },
          { type: 'email', message: '请输入正确的邮箱格式', trigger: ['blur', 'change'] }
        ],
      },
      showLogin: true, // 设置初始值为true以显示登录表单
    }
  },
  methods: {
    async getVerificationCode() {
      try {
        // 发送获取验证码的请求
        const response = await this.$axios.post('/api/getMailVerifyCode', {
          mail: this.registerForm.email,
          username: this.registerForm.username,
        });
        this.$message.success('验证码已发送至您的邮箱，请查收！');
      } catch (error) {
        console.error('Error getting verification code:', error);
      }
    },
    async register() {
      try {
        const response = await this.$axios.post('/api/register', {
          username: this.registerForm.username,
          password: this.registerForm.password,
          email: this.registerForm.email,
          verificationCode: this.registerForm.verificationCode
        })
        if (response.data.code == 200) {
          this.$message({
            type: 'success',
            message: response.data.msg
          });
          await this.$router.push('/login'); // 跳转到注册页面
        } else {
          this.$message({
            type: 'error',
            message: response.data.msg
          });
        }
      } catch (error) {
        console.error('Error login:', error)
      }
    }
  }
};
</script>