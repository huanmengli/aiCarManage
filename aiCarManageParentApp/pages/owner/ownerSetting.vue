<template>
  <view class="safe-container">
    <!-- 姓名 -->
    <view class="edit-item">
      <text class="label">真实姓名</text>
      <input class="inp" v-model="userInfo.realName" placeholder="请输入姓名" />
    </view>

    <!-- 手机号 -->
    <view class="edit-item">
      <text class="label">绑定手机号</text>
      <input class="inp" v-model="userInfo.phone" placeholder="请输入手机号" type="number" maxlength="11" />
    </view>

    <!-- 修改密码区域 -->
    <view class="pwd-box">
      <view class="title">修改登录密码</view>
      <view class="edit-item">
        <text class="label">原密码</text>
        <input class="inp" v-model="pwdForm.oldPwd" placeholder="请输入原密码" password />
      </view>
      <view class="edit-item">
        <text class="label">新密码</text>
        <input class="inp" v-model="pwdForm.newPwd" placeholder="请输入新密码" password />
      </view>
      <view class="edit-item">
        <text class="label">确认密码</text>
        <input class="inp" v-model="pwdForm.confirmPwd" placeholder="再次输入新密码" password />
      </view>
    </view>

    <!-- 统一保存按钮 -->
    <view class="save-btn" @click="saveAllInfo">
      <text>保存全部修改</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      userInfo: {},
      pwdForm: {
        oldPwd: '',
        newPwd: '',
        confirmPwd: ''
      }
    }
  },
  onLoad() {
    this.userInfo = uni.getStorageSync('userInfo') || {}
  },
  methods: {
    saveAllInfo() {
      // 校验姓名
      if (!this.userInfo.realName.trim()) {
        uni.showToast({ title: '请填写真实姓名', icon: 'none' })
        return
      }
      // 校验手机号
      let phoneReg = /^1[3-9]\d{9}$/
      if (!phoneReg.test(this.userInfo.phone)) {
        uni.showToast({ title: '手机号格式错误', icon: 'none' })
        return
      }

      // 密码非空才校验密码
      if (this.pwdForm.oldPwd || this.pwdForm.newPwd || this.pwdForm.confirmPwd) {
        if (!this.pwdForm.oldPwd) {
          uni.showToast({ title: '请输入原密码', icon: 'none' })
          return
        }
        if (this.pwdForm.newPwd.length < 6) {
          uni.showToast({ title: '新密码至少6位', icon: 'none' })
          return
        }
        if (this.pwdForm.newPwd !== this.pwdForm.confirmPwd) {
          uni.showToast({ title: '两次密码不一致', icon: 'none' })
          return
        }
        // 此处可调用后端修改密码接口
      }

      // 保存用户信息到本地缓存
      uni.setStorageSync('userInfo', this.userInfo)
      uni.showToast({ title: '修改成功' })
      setTimeout(() => {
        uni.navigateBack()
      }, 1200)
    }
  }
}
</script>

<style scoped>
.safe-container {
  background-color: #f5f7fa;
  min-height: 100vh;
  padding: 30rpx;
}
.edit-item {
  display: flex;
  align-items: center;
  background: #fff;
  padding: 0 30rpx;
  height: 90rpx;
  border-radius: 16rpx;
  margin-bottom: 20rpx;
}
.label {
  font-size: 30rpx;
  color: #333;
  width: 160rpx;
}
.inp {
  flex: 1;
  font-size: 30rpx;
  color: #111;
}
.pwd-box {
  margin-top: 30rpx;
}
.title {
  font-size: 32rpx;
  color: #333;
  margin-bottom: 20rpx;
  padding-left: 10rpx;
}
.save-btn {
  margin-top: 60rpx;
  height: 90rpx;
  line-height: 90rpx;
  text-align: center;
  background: #2563eb;
  color: #fff;
  border-radius: 16rpx;
  font-size: 32rpx;
}
</style>