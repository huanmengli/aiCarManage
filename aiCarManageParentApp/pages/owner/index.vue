<template>
  <view class="mine-container">
    <!-- 用户信息卡片 -->
    <view class="user-card">
      <view class="avatar-wrap">
        <image src="/static/avatar.png" class="avatar" mode="aspectFill" />
        <view class="cert-icon">
          <image src="/static/cert.png" class="cert-img" mode="aspectFit" />
        </view>
      </view>
      <view class="user-info">
        <view class="name-row">
          <text class="user-name">张晓明 爸爸</text>
        </view>
        <text class="phone-text">138 **** 8000</text>
        <view class="tags-row">
          <view class="tag">实名家长</view>
          <view class="tag class-tag">三年级2班</view>
        </view>
      </view>
      <view class="arrow-right">
        <image src="/static/arrow.png" class="arrow-icon" mode="aspectFit" />
      </view>
    </view>

    <!-- 车辆管理 -->
    <view class="section-wrap">
      <view class="section-header">
        <text class="section-title">车辆管理</text>
        <view class="add-btn" @click="goAddCar">
          <text class="add-text">+ 添加</text>
        </view>
      </view>
      <view class="item-card" @click="goCarDetail">
        <view class="item-icon-wrap">
          <image src="/static/car.png" class="item-icon" mode="aspectFit" />
        </view>
        <view class="item-content">
          <text class="item-title">粤B·88888</text>
          <text class="item-desc">黑色·特斯拉 Model 3</text>
        </view>
        <view class="arrow-right">
          <image src="/static/arrow.png" class="arrow-icon" mode="aspectFit" />
        </view>
      </view>
    </view>

    <!-- 接送人管理 -->
    <view class="section-wrap">
      <view class="section-header">
        <text class="section-title">接送人管理</text>
        <view class="add-btn" @click="goAddReceiver">
          <text class="add-text">+ 添加</text>
        </view>
      </view>
      <view class="item-card" @click="goReceiverDetail">
        <image src="/static/receiver.png" class="receiver-avatar" mode="aspectFill" />
        <view class="item-content">
          <text class="item-title">王美兰</text>
          <text class="item-desc">关系：奶奶 · 139 **** 1234</text>
        </view>
        <view class="arrow-right">
          <image src="/static/arrow.png" class="arrow-icon" mode="aspectFit" />
        </view>
      </view>
    </view>

    <!-- 功能列表 -->
    <view class="section-wrap">
      <view class="func-item" @click="goSafe">
        <view class="func-icon-wrap">
          <image src="/static/safe.png" class="func-icon" mode="aspectFit" />
        </view>
        <text class="func-title">账号与安全</text>
        <view class="arrow-right">
          <image src="/static/arrow.png" class="arrow-icon" mode="aspectFit" />
        </view>
      </view>
      <view class="func-item border-top" @click="goRecord">
        <view class="func-icon-wrap">
          <image src="/static/record.png" class="func-icon" mode="aspectFit" />
        </view>
        <text class="func-title">接送记录</text>
        <view class="arrow-right">
          <image src="/static/arrow.png" class="arrow-icon" mode="aspectFit" />
        </view>
      </view>
    </view>

    <!-- 退出登录按钮 -->
    <view class="logout-btn" @click="handleLogout">
      <image src="/static/logout.png" class="logout-icon" mode="aspectFit" />
      <text class="logout-text">退出登录</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    // 去添加车辆
    goAddCar() {
      uni.navigateTo({ url: "/pages/car/add" });
    },
    // 车辆详情
    goCarDetail() {
      uni.navigateTo({ url: "/pages/car/detail" });
    },
    // 去添加接送人
    goAddReceiver() {
      uni.navigateTo({ url: "/pages/receiver/add" });
    },
    // 接送人详情
    goReceiverDetail() {
      uni.navigateTo({ url: "/pages/receiver/detail" });
    },
    // 账号与安全
    goSafe() {
      uni.navigateTo({ url: "/pages/mine/safe" });
    },
    // 接送记录
    goRecord() {
      uni.navigateTo({ url: "/pages/mine/record" });
    },
    // 退出登录
    handleLogout() {
      uni.showModal({
        title: "提示",
        content: "确定要退出登录吗？",
        success: (res) => {
          if (res.confirm) {
            uni.removeStorageSync("token");
            uni.removeStorageSync("userInfo");
            uni.reLaunch({ url: "/pages/login/parent" });
          }
        }
      });
    }
  }
};
</script>

<style scoped>
.mine-container {
  background-color: #f5f7fa;
  min-height: 100vh;
}

/* 用户信息卡片 */
.user-card {
  background: #ffffff;
  display: flex;
  align-items: center;
  padding: 40rpx 30rpx;
}
.avatar-wrap {
  position: relative;
}
.avatar {
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
}
.cert-icon {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 40rpx;
  height: 40rpx;
  background: #2563eb;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.cert-img {
  width: 24rpx;
  height: 24rpx;
}
.user-info {
  flex: 1;
  margin-left: 24rpx;
}
.user-name {
  font-size: 36rpx;
  font-weight: bold;
  color: #1a1a1a;
}
.phone-text {
  font-size: 28rpx;
  color: #666;
  margin: 8rpx 0;
}
.tags-row {
  display: flex;
  gap: 16rpx;
  margin-top: 8rpx;
}
.tag {
  background: #e6f0ff;
  color: #2563eb;
  font-size: 24rpx;
  padding: 6rpx 16rpx;
  border-radius: 20rpx;
}
.class-tag {
  background: #f0f2f5;
  color: #666;
}
.arrow-right {
  width: 40rpx;
  height: 40rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}
.arrow-icon {
  width: 20rpx;
  height: 20rpx;
}

/* 通用模块 */
.section-wrap {
  margin: 24rpx 30rpx 0;
  background: #fff;
  border-radius: 20rpx;
  overflow: hidden;
}
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30rpx;
}
.section-title {
  font-size: 32rpx;
  color: #333;
  font-weight: 500;
}
.add-btn {
  display: flex;
  align-items: center;
}
.add-text {
  font-size: 28rpx;
  color: #2563eb;
}
.item-card {
  display: flex;
  align-items: center;
  padding: 0 30rpx 30rpx;
}
.item-icon-wrap {
  width: 80rpx;
  height: 80rpx;
  background: #f0f6ff;
  border-radius: 16rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}
.item-icon {
  width: 40rpx;
  height: 40rpx;
}
.receiver-avatar {
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
}
.item-content {
  flex: 1;
  margin-left: 20rpx;
}
.item-title {
  display: block;
  font-size: 32rpx;
  color: #1a1a1a;
  margin-bottom: 6rpx;
}
.item-desc {
  font-size: 26rpx;
  color: #666;
}

/* 功能列表 */
.func-item {
  display: flex;
  align-items: center;
  padding: 30rpx;
}
.border-top {
  border-top: 1rpx solid #f0f2f5;
}
.func-icon-wrap {
  width: 72rpx;
  height: 72rpx;
  background: #f0f6ff;
  border-radius: 16rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}
.func-icon {
  width: 36rpx;
  height: 36rpx;
}
.func-title {
  flex: 1;
  font-size: 32rpx;
  color: #1a1a1a;
  margin-left: 20rpx;
}

/* 退出登录 */
.logout-btn {
  margin: 60rpx 30rpx;
  background: #fff;
  border: 1rpx solid #ff4d4f;
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 28rpx 0;
}
.logout-icon {
  width: 32rpx;
  height: 32rpx;
  margin-right: 12rpx;
}
.logout-text {
  font-size: 32rpx;
  color: #ff4d4f;
}
</style>