<template>
  <view class="mine-wrap">
    <!-- 顶部渐变个人信息卡片 点击进入修改 -->
    <view class="user-card" @click="goEditUser">
      <view class="user-left">
        <image src="/static/teacher.png" class="user-avatar"></image>
        <view class="user-info">
          <text class="user-name">张老师</text>
          <text class="user-phone">13800001111</text>
        </view>
      </view>
      <view class="arrow-right">
        <text class="edit-txt">编辑资料</text>
        <image src="/static/right.png" class="right-icon"></image>
      </view>
    </view>

    <!-- 统计卡片：班级数 + 学生数 -->
    <view class="stat-card">
      <view class="stat-item">
        
        <text class="stat-label">管理班级</text>
		<text class="stat-num">3</text>
      </view>
      <view class="stat-line"></view>
      <view class="stat-item">
        
        <text class="stat-label">管理学生</text>
		<text class="stat-num">126</text>
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

    <!-- 退出登录 -->
    <view class="logout-btn" @click="logout">
      <text>退出登录</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    // 前往个人信息修改
    goEditUser() {
      uni.navigateTo({
        url:'/pages/teacher/editUser'
      })
    },
    goClass(){
      uni.navigateTo({url:'/pages/teacher/class'})
    },
    goNotice(){
      uni.navigateTo({url:'/pages/teacher/notice'})
    },
    goRoad(){
      uni.navigateTo({url:'/pages/teacher/road'})
    },
    // 退出登录
    logout(){
      uni.showModal({
        title:'温馨提示',
        content:'确定要退出当前账号吗？',
        confirmColor:'#2563eb',
        success:res=>{
          if(res.confirm){
            uni.clearStorageSync()
            uni.reLaunch({
              url:'/pages/login/teacherLogin'
            })
          }
        }
      })
    }
  }
}
</script>

<style scoped>
.mine-wrap{
  background-color: #f5f7fa;
  min-height: 100vh;
  padding: 30rpx;
}
/* 顶部用户卡片 */
.user-card{
  background: linear-gradient(135deg,#2563eb,#4080ff);
  border-radius: 24rpx;
  padding: 40rpx 30rpx;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 10rpx 30rpx rgba(37,99,235,0.25);
  margin-bottom: 20rpx;
}
.user-left{
  display: flex;
  align-items: center;
}
.user-avatar{
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
  border: 3rpx solid #ffffff;
}
.user-info{
  margin-left: 25rpx;
}
.user-name{
  font-size: 38rpx;
  color: #fff;
  font-weight: 500;
  display: block;
}
.user-phone{
  font-size: 26rpx;
  color: rgba(255,255,255,0.85);
  margin-top: 8rpx;
}
.arrow-right{
  display: flex;
  align-items: center;
}
.edit-txt{
  font-size: 26rpx;
  color: #fff;
  margin-right: 8rpx;
}
.right-icon{
  width: 16rpx;
  height: 28rpx;
}

/* 统计卡片：班级 + 学生 */
.stat-card{
  background: #fff;
  border-radius: 20rpx;
  display: flex;
  padding: 30rpx 0;
  margin-bottom: 30rpx;
  box-shadow: 0 4rpx 12rpx rgba(0,0,0,0.04);
}
.stat-item{
  flex:1;
  text-align: center;
}
.stat-num{
  font-size: 42rpx;
  font-weight: bold;
  color: #2563eb;
  line-height: 1.2;
}
.stat-label{
  font-size: 26rpx;
  color: #666;
}
.stat-line{
  width:1rpx;
  height:50rpx;
  background:#eee;
  align-self: center;
}

/* 功能列表 */
.func-box{
  background-color: #ffffff;
  border-radius: 20rpx;
  overflow: hidden;
}
.func-item{
  display: flex;
  align-items: center;
  padding: 35rpx 30rpx;
  border-bottom: 1rpx solid #f2f3f5;
}
.func-item:last-child{
  border-bottom: none;
}
.func-icon{
  width: 76rpx;
  height: 76rpx;
  border-radius: 18rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}
.bg-blue{ background-color: #e8f1ff; }
.bg-orange{ background-color: #fff3e8; }
.bg-green{ background-color: #e8faf0; }
.icon-img{ width: 40rpx; height: 40rpx; }
.func-name{
  flex: 1;
  font-size: 32rpx;
  color: #333;
  margin-left: 22rpx;
}
.item-arrow{ width: 16rpx; height: 28rpx; }

/* 退出登录 */
.logout-btn{
  margin-top: 80rpx;
  background-color: #fff;
  text-align: center;
  line-height: 90rpx;
  border-radius: 18rpx;
  font-size: 32rpx;
  color: #f53f3f;
}
</style>