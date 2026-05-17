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
      // const { phone, password } = this.formData;
      // if (!phone || phone.length !== 11) {
      //   uni.showToast({ title: "请输入正确手机号", icon: "none" });
      //   return;
      // }
      // if (!password) {
      //   uni.showToast({ title: "请输入密码", icon: "none" });
      //   return;
      // }

      // uni.showLoading({ title: "登录中..." });
      // uni.request({
      //   url: "你的教师登录接口地址",
      //   method: "POST",
      //   data: { phone, password },
      //   success: (res) => {
      //     uni.hideLoading();
      //     if (res.data.code === 200) {
      //       uni.showToast({ title: "登录成功" });
      //       uni.setStorageSync("teacherToken", res.data.data.token);
      //       uni.switchTab({ url: "/pages/teacher/index" });
      //     } else {
      //       uni.showToast({ title: res.data.msg || "登录失败", icon: "none" });
      //     }
      //   },
      //   fail: () => {
      //     uni.hideLoading();
      //     uni.showToast({ title: "网络异常", icon: "none" });
      //   }
      // });
    }
  }
};
</script>

<style scoped>
.login-container {
  padding: 40rpx 60rpx;
  background: #fff;
  min-height: 100vh;
  box-sizing: border-box;
}

.back-btn {
  width: 60rpx;
  height: 60rpx;
  background: #f5f5f5;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.back-icon {
  width: 30rpx;
  height: 30rpx;
}

.title-section {
  margin-top: 60rpx;
  margin-bottom: 80rpx;
}
.main-title {
  font-size: 48rpx;
  font-weight: bold;
  color: #1a1a1a;
  display: block;
  margin-bottom: 20rpx;
}
.sub-title {
  font-size: 30rpx;
  color: #666;
}

.form-section {
  width: 100%;
}
.input-item {
  display: flex;
  align-items: center;
  background: #f8f9fa;
  border-radius: 16rpx;
  padding: 0 30rpx;
  height: 100rpx;
  margin-bottom: 30rpx;
}
.input-icon {
  width: 32rpx;
  height: 32rpx;
  margin-right: 20rpx;
}
.input-field {
  flex: 1;
  font-size: 30rpx;
  color: #333;
}
.input-field::placeholder {
  color: #999;
}
.eye-icon {
  width: 32rpx;
  height: 32rpx;
}

.login-btn {
  width: 100%;
  height: 100rpx;
  background: #2563eb;
  color: #fff;
  border-radius: 16rpx;
  font-size: 34rpx;
  font-weight: 500;
  border: none;
  box-shadow: 0 8rpx 20rpx rgba(37, 99, 235, 0.2);
  margin: 40rpx 0 60rpx;
}
</style>