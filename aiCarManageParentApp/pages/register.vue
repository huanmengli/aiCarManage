<template>
  <view class="register-container">
    
    <!-- 标题 -->
    <view class="title-section">
      <text class="main-title">注册账号</text>
      <text class="sub-title">创建一个新账号以开始使用</text>
    </view>

    <!-- 表单区域 -->
    <view class="form-section">
      <!-- 手机号 -->
      <view class="input-item">
        <input
          type="number"
          placeholder="手机号"
          v-model="formData.phone"
          class="input-field"
          maxlength="11"
        />
      </view>

      <!-- 设置密码 -->
      <view class="input-item">
        <input
          :type="pwdType1"
          placeholder="设置密码"
          v-model="formData.password"
          class="input-field"
        />
        <image
          :src="pwdType1 === 'password' ? '/static/eye_close.png' : '/static/eye_open.png'"
          mode="aspectFit"
          class="eye-icon"
          @click="togglePwd1"
        ></image>
      </view>

      <!-- 确认密码 -->
      <view class="input-item">
        <input
          :type="pwdType2"
          placeholder="确认密码"
          v-model="formData.confirmPwd"
          class="input-field"
        />
        <image
          :src="pwdType2 === 'password' ? '/static/eye_close.png' : '/static/eye_open.png'"
          mode="aspectFit"
          class="eye-icon"
          @click="togglePwd2"
        ></image>
      </view>

      <!-- 注册按钮 -->
      <button class="register-btn" @click="handleRegister">完成注册</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        phone: "",
        password: "",
        confirmPwd: ""
      },
      pwdType1: "password",
      pwdType2: "password"
    };
  },
  methods: {
    // 返回上一页
    goBack() {
      uni.navigateBack();
    },
    // 切换设置密码可见
    togglePwd1() {
      this.pwdType1 = this.pwdType1 === "password" ? "text" : "password";
    },
    // 切换确认密码可见
    togglePwd2() {
      this.pwdType2 = this.pwdType2 === "password" ? "text" : "password";
    },
    // 注册逻辑
    handleRegister() {
      const { phone, password, confirmPwd } = this.formData;

      // 表单校验
      if (!phone || phone.length !== 11) {
        uni.showToast({ title: "请输入正确手机号", icon: "none" });
        return;
      }
      if (!password || password.length < 6) {
        uni.showToast({ title: "密码长度不能少于6位", icon: "none" });
        return;
      }
      if (password !== confirmPwd) {
        uni.showToast({ title: "两次密码不一致", icon: "none" });
        return;
      }

      // 调用注册接口
      uni.showLoading({ title: "注册中..." });
      uni.request({
        url: "https://你的后端接口/register",
        method: "POST",
        data: { phone, password },
        success: (res) => {
          uni.hideLoading();
          if (res.data.code === 200) {
            uni.showToast({ title: "注册成功" });
            // 注册成功跳回登录页
            uni.navigateBack();
          } else {
            uni.showToast({ title: res.data.msg || "注册失败", icon: "none" });
          }
        },
        fail: () => {
          uni.hideLoading();
          uni.showToast({ title: "网络异常", icon: "none" });
        }
      });
    }
  }
};
</script>

<style scoped>
.register-container {
  padding: 5.33vw 4vw;
  background: #fff;
  min-height: 100vh;
  box-sizing: border-box;
}

/* 返回按钮 */
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

/* 标题区域 */
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

/* 表单 */
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

/* 注册按钮 */
.register-btn {
  width: 100%;
  height: 13.33vw;
  background: #2563eb;
  color: #fff;
  border-radius: 2.13vw;
  font-size: 4.5vw;
  font-weight: 500;
  border: none;
  box-shadow: 0 1.06vw 2.66vw rgba(37, 99, 235, 0.2);
  margin-top: 2.6vw;
}
</style>