<style scoped>
.container {
  padding: 16rpx;
  background-color: #f5f7fa;
  min-height: 100vh;
}
.detail-card {
  background-color: #fff;
  border-radius: 12rpx;
  padding: 32rpx;
}
.detail-title {
  font-size: 36rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 24rpx;
}
.detail-meta {
  font-size: 26rpx;
  color: #999;
  margin-bottom: 32rpx;
}
.detail-content {
  font-size: 30rpx;
  color: #333;
  line-height: 1.8;
}
</style>

<template>
  <view class="container">
    <view class="detail-card">
      <view class="detail-title">{{ detail.title }}</view>
      <view class="detail-meta">发布时间：{{ detail.time }} &nbsp;&nbsp; 类型：{{ detail.type }}</view>
      <view class="detail-content">{{ detail.content }}</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      detail: {
        title: '2026年春季运动会通知',
        time: '2026-05-15 10:20',
        type: '全校通知',
        content: '为增强学生体质，丰富校园文化生活，学校定于2026年5月20日（星期三）上午8:30在操场举办春季运动会。请各班提前组织报名、训练，运动会当天统一穿校服，准时到场参加开幕式。'
      }
    };
  }
};
</script>