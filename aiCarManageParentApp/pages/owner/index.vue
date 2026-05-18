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
      uni.navigateTo({ url: "/pages/" });
    },
    // 车辆详情
    goCarDetail() {
      uni.navigateTo({ url: "/pages/owner/ownerAndCar" });
    },
    // 去添加接送人
    goAddReceiver() {
      uni.navigateTo({ url: "/pages/" });
    },
    // 接送人详情
    goReceiverDetail() {
      uni.navigateTo({ url: "/pages/owner/ownerAndPickup" });
    },
    // 账号与安全
    goSafe() {
      uni.navigateTo({ url: "/pages/owner/ownerSetting" });
    },
    // 接送记录
    goRecord() {
      uni.navigateTo({ url: "/pages/owner/ownAndRecord" });
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
            uni.reLaunch({ url: "/pages/login" });
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
  padding: 5.33vw 4vw;
}
.avatar-wrap {
  position: relative;
}
.avatar {
  width: 16vw;
  height: 16vw;
  border-radius: 50%;
}
.cert-icon {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 5.33vw;
  height: 5.33vw;
  background: #2563eb;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.cert-img {
  width: 3.2vw;
  height: 3.2vw;
}
.user-info {
  flex: 1;
  margin-left: 3.2vw;
}
.user-name {
  font-size: 4.8vw;
  font-weight: bold;
  color: #1a1a1a;
}
.phone-text {
  font-size: 3.73vw;
  color: #666;
  margin: 1.06vw 0;
}
.tags-row {
  display: flex;
  gap: 2.13vw;
  margin-top: 1.06vw;
}
.tag {
  background: #e6f0ff;
  color: #2563eb;
  font-size: 3.2vw;
  padding: 0.8vw 2.13vw;
  border-radius: 2.66vw;
}
.class-tag {
  background: #f0f2f5;
  color: #666;
}
.arrow-right {
  width: 5.33vw;
  height: 5.33vw;
  display: flex;
  align-items: center;
  justify-content: center;
}
.arrow-icon {
  width: 2.66vw;
  height: 2.66vw;
}

/* 通用模块 */
.section-wrap {
  margin: 3.2vw 4vw 0;
  background: #fff;
  border-radius: 2.66vw;
  overflow: hidden;
}
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 4vw;
}
.section-title {
  font-size: 4.26vw;
  color: #333;
  font-weight: 500;
}
.add-btn {
  display: flex;
  align-items: center;
}
.add-text {
  font-size: 3.73vw;
  color: #2563eb;
}
.item-card {
  display: flex;
  align-items: center;
  padding: 0 4vw 4vw;
}
.item-icon-wrap {
  width: 10.66vw;
  height: 10.66vw;
  background: #f0f6ff;
  border-radius: 2.13vw;
  display: flex;
  align-items: center;
  justify-content: center;
}
.item-icon {
  width: 5.33vw;
  height: 5.33vw;
}
.receiver-avatar {
  width: 10.66vw;
  height: 10.66vw;
  border-radius: 50%;
}
.item-content {
  flex: 1;
  margin-left: 2.66vw;
}
.item-title {
  display: block;
  font-size: 4.26vw;
  color: #1a1a1a;
  margin-bottom: 0.8vw;
}
.item-desc {
  font-size: 3.46vw;
  color: #666;
}

/* 功能列表 */
.func-item {
  display: flex;
  align-items: center;
  padding: 4vw;
}
.border-top {
  border-top: 1rpx solid #f0f2f5;
}
.func-icon-wrap {
  width: 9.6vw;
  height: 9.6vw;
  background: #f0f6ff;
  border-radius: 2.13vw;
  display: flex;
  align-items: center;
  justify-content: center;
}
.func-icon {
  width: 4.8vw;
  height: 4.8vw;
}
.func-title {
  flex: 1;
  font-size: 4.26vw;
  color: #1a1a1a;
  margin-left: 2.66vw;
}

/* 退出登录 */
.logout-btn {
  margin: 8vw 4vw;
  background: #fff;
  border: 1rpx solid #ff4d4f;
  border-radius: 2.66vw;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 3.73vw 0;
}
.logout-icon {
  width: 4.26vw;
  height: 4.26vw;
  margin-right: 1.6vw;
}
.logout-text {
  font-size: 4.26vw;
  color: #ff4d4f;
}
</style>