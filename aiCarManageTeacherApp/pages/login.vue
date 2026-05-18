<template>
  <view class="login-container">
   
    <!-- 标题 -->
    <view class="title-section">
      <text class="main-title">教师登录</text>
      <text class="sub-title">欢迎回来，请登录您的账号</text>
    </view>

    <!-- 表单 -->
    <view class="form-section">
      <!-- 手机号 -->
      <view class="input-item">
        <image src="/static/phone.png" mode="aspectFit" class="input-icon"></image>
        <input
          type="number"
          placeholder="请输入手机号"
          v-model="formData.phone"
          class="input-field"
          maxlength="11"
        />
      </view>

      <!-- 密码 -->
      <view class="input-item">
        <image src="/static/password.png" mode="aspectFit" class="input-icon"></image>
        <input
          :type="pwdType"
          placeholder="请输入密码"
          v-model="formData.password"
          class="input-field"
        />
        <image
          :src="pwdType === 'password' ? '/static/eye_close.png' : '/static/eye_open.png'"
          mode="aspectFit"
          class="eye-icon"
          @click="togglePwd"
        ></image>
      </view>

      <!-- 登录按钮 -->
      <button class="login-btn" @click="handleLogin">立即登录</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        phone: "",
        password: ""
      },
      pwdType: "password"
    };
  },
  methods: {
    // 返回上一页
    goBack() {
      uni.navigateBack();
    },
    // 切换密码显隐
    togglePwd() {
      this.pwdType = this.pwdType === "password" ? "text" : "password";
    },
    // 教师登录
    handleLogin() {
		uni.switchTab({ url: "/pages/owner/index" });
    }
  }
};
</script>

<style scoped>
.login-container {
  padding: 5.33vw 4vw;
  background: #fff;
  min-height: 100vh;
  box-sizing: border-box;
}

.back-btn {
  width: 4vw;
  height: 4vw;
  background: #f5f5f5;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.back-icon {
  width: 2vw;
  height: 2vw;
}

.title-section {
  margin-top: 8vw;
  margin-bottom: 10vw;
}
.main-title {
  font-size: 6.4vw;
  font-weight: bold;
  color: #1a1a1a;
  display: block;
  margin-bottom: 2.6vw;
}
.sub-title {
  font-size: 4vw;
  color: #666;
}

.form-section {
  width: 100%;
}
.input-item {
  display: flex;
  align-items: center;
  background: #f8f9fa;
  border-radius: 2.13vw;
  padding: 0 4vw;
  height: 13.33vw;
  margin-bottom: 4vw;
}
.input-icon {
  width: 4.26vw;
  height: 4.26vw;
  margin-right: 2.6vw;
}
.input-field {
  flex: 1;
  font-size: 4vw;
  color: #333;
}
.input-field::placeholder {
  color: #999;
}
.eye-icon {
  width: 4.26vw;
  height: 4.26vw;
}

.login-btn {
  width: 100%;
  height: 13.33vw;
  background: #2563eb;
  color: #fff;
  border-radius: 2.13vw;
  font-size: 4.5vw;
  font-weight: 500;
  border: none;
  box-shadow: 0 1.06vw 2.66vw rgba(37, 99, 235, 0.2);
  margin: 5.33vw 0 8vw;
}
</style>