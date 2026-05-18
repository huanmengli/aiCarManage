<template>
  <view class="mine-wrap">
    <!-- 顶部渐变个人信息卡片 点击进入修改 -->
    <view class="user-card" @click="goEditUser">
      <view class="user-left">
        <image src="/static/teacher.png" class="user-avatar"></image>
        <view class="user-info">
          <text class="user-name">{{ userInfo.name }}</text>
          <text class="user-phone">{{ userInfo.phone }}</text>
        </view>
      </view>
      <view class="arrow-right">
        <text class="edit-txt">编辑资料</text>
        <image src="/static/right.png" class="right-icon"></image>
      </view>
    </view>

    <!-- 统计卡片：可点击跳转 + 动态数据 -->
    <view class="stat-card">
      <view class="stat-item" @click="goClass">
        <text class="stat-num">{{ classCount }}</text>
        <text class="stat-label">管理班级</text>
      </view>
      <view class="stat-line"></view>
      <view class="stat-item" @click="goStudent">
        <text class="stat-num">{{ studentCount }}</text>
        <text class="stat-label">管理学生</text>
      </view>
    </view>

    <!-- 功能列表 -->
    <view class="func-box">
      <view class="func-item" @click="goClass">
        <view class="func-icon bg-blue">
          <image src="/static/class.png" class="icon-img"></image>
        </view>
        <text class="func-name">班级管理</text>
        <image src="/static/right.png" class="item-arrow"></image>
      </view>
      <view class="func-item" @click="goNotice">
        <view class="func-icon bg-orange">
          <image src="/static/notice.png" class="icon-img"></image>
        </view>
        <text class="func-name">通知管理</text>
        <image src="/static/right.png" class="item-arrow"></image>
      </view>
      <view class="func-item" @click="goRoad">
        <view class="func-icon bg-green">
          <image src="/static/road.png" class="icon-img"></image>
        </view>
        <text class="func-name">车道信息管理</text>
        <image src="/static/right.png" class="item-arrow"></image>
      </view>
    </view>

    <!-- 底部固定退出登录 -->
    <view class="logout-btn" @click="logout">
      <text>退出登录</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      // 用户信息（可从接口获取）
      userInfo: {
        name: "张老师",
        phone: "13800001111"
      },
      // 动态班级数、学生数
      classCount: 3,
      studentCount: 126
    };
  },
  onLoad() {
    // 你可以在这里请求后端接口给 classCount、studentCount 赋值
    // this.getClassData()
  },
  methods: {
    // 去个人信息修改
    goEditUser() {
      uni.navigateTo({
        url: "/pages/owner/ownerSetting"
      });
    },
    // 班级管理
    goClass() {
      uni.navigateTo({
        url: "/pages/owner/ownerAndClass"
      });
    },
    // 学生管理（新增跳转）
    goStudent() {
      uni.navigateTo({
        url: "/pages/owner/classAndStudent"
      });
    },
    // 通知管理
    goNotice() {
      uni.navigateTo({
        url: "/pages/owner/ownerAndNotice"
      });
    },
    // 车道管理
    goRoad() {
      uni.navigateTo({
        url: "/pages/owner/ownerAndCar"
      });
    },
    // 退出登录
    logout() {
      uni.showModal({
        title: "温馨提示",
        content: "确定要退出当前账号吗？",
        confirmColor: "#2563eb",
        success: (res) => {
          if (res.confirm) {
            uni.clearStorageSync();
            uni.reLaunch({
              url: "/pages/login"
            });
          }
        }
      });
    }
  }
};
</script>

<style scoped>
.mine-wrap {
  background-color: #f5f7fa;
  min-height: 100vh;
  padding: 4vw;
  padding-bottom: 16vw;
}

/* 顶部用户卡片 */
.user-card {
  background: linear-gradient(135deg, #2563eb, #4080ff);
  border-radius: 3.2vw;
  padding: 5.33vw 4vw;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 1.33vw 4vw rgba(37, 99, 235, 0.25);
  margin-bottom: 2.66vw;
}
.user-left {
  display: flex;
  align-items: center;
}
.user-avatar {
  width: 16vw;
  height: 16vw;
  border-radius: 50%;
  border: 0.4vw solid #ffffff;
}
.user-info {
  margin-left: 3.33vw;
}
.user-name {
  font-size: 5.06vw;
  color: #fff;
  font-weight: 500;
  display: block;
}
.user-phone {
  font-size: 3.46vw;
  color: rgba(255, 255, 255, 0.85);
  margin-top: 1.06vw;
}
.arrow-right {
  display: flex;
  align-items: center;
}
.edit-txt {
  font-size: 3.46vw;
  color: #fff;
  margin-right: 1.06vw;
}
.right-icon {
  width: 2.13vw;
  height: 3.73vw;
}

/* 统计卡片（可点击） */
.stat-card {
  background: #fff;
  border-radius: 2.66vw;
  display: flex;
  padding: 4vw 0;
  margin-bottom: 4vw;
  box-shadow: 0 0.53vw 1.6vw rgba(0, 0, 0, 0.04);
}
.stat-item {
  flex: 1;
  text-align: center;
}
.stat-num {
  font-size: 5.6vw;
  font-weight: bold;
  color: #2563eb;
  line-height: 1.2;
}
.stat-label {
  font-size: 3.46vw;
  color: #666;
}
.stat-line {
  width: 0.13vw;
  height: 6.66vw;
  background: #eee;
  align-self: center;
}

/* 功能列表 */
.func-box {
  background-color: #ffffff;
  border-radius: 2.66vw;
  overflow: hidden;
}
.func-item {
  display: flex;
  align-items: center;
  padding: 4.66vw 4vw;
  border-bottom: 0.13vw solid #f2f3f5;
}
.func-item:last-child {
  border-bottom: none;
}
.func-icon {
  width: 10.13vw;
  height: 10.13vw;
  border-radius: 2.4vw;
  display: flex;
  align-items: center;
  justify-content: center;
}
.bg-blue { background-color: #e8f1ff; }
.bg-orange { background-color: #fff3e8; }
.bg-green { background-color: #e8faf0; }
.icon-img { width: 5.33vw; height: 5.33vw; }
.func-name {
  flex: 1;
  font-size: 4.26vw;
  color: #333;
  margin-left: 2.93vw;
}
.item-arrow { width: 2.13vw; height: 3.73vw; }

/* 底部固定退出按钮 */
.logout-btn {
  position: fixed;
  bottom: 5.33vw;
  left: 4vw;
  right: 4vw;
  background-color: #fff;
  text-align: center;
  line-height: 12vw;
  border-radius: 2.4vw;
  font-size: 4.26vw;
  color: #f53f3f;
  box-shadow: 0 0.53vw 2vw rgba(245, 63, 63, 0.15);
}
</style>