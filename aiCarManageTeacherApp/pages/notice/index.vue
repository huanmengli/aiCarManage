<style scoped>
.container {
  padding: 16rpx;
  background-color: #f5f7fa;
  min-height: 100vh;
}
.tab-bar {
  display: flex;
  background-color: #fff;
  border-radius: 12rpx;
  margin-bottom: 16rpx;
}
.tab-item {
  flex: 1;
  text-align: center;
  padding: 24rpx 0;
  font-size: 30rpx;
}
.tab-item.active {
  color: #1677ff;
  border-bottom: 4rpx solid #1677ff;
}
.form-card {
  background-color: #fff;
  border-radius: 12rpx;
  padding: 24rpx;
}
.form-item {
  margin-bottom: 24rpx;
}
.form-label {
  font-size: 28rpx;
  color: #333;
  margin-bottom: 12rpx;
}
.input-box {
  width: 100%;
  border: 1rpx solid #eee;
  border-radius: 8rpx;
  padding: 20rpx;
  font-size: 28rpx;
  box-sizing: border-box;
}
.textarea {
  min-height: 200rpx;
}
.btn-submit {
  width: 100%;
  background-color: #1677ff;
  color: #fff;
  text-align: center;
  padding: 24rpx 0;
  border-radius: 12rpx;
  font-size: 32rpx;
}
.select-box {
  display: flex;
  flex-wrap: wrap;
  gap: 16rpx;
}
.select-tag {
  background-color: #f0f7ff;
  color: #1677ff;
  padding: 8rpx 16rpx;
  border-radius: 8rpx;
  font-size: 26rpx;
}
</style>

<template>
  <view class="container">
    <!-- 切换：按年级 / 按班级 -->
    <view class="tab-bar">
      <view class="tab-item" :class="{active: activeTab === 'grade'}">按年级通知</view>
      <view class="tab-item" :class="{active: activeTab === 'class'}">按班级通知</view>
    </view>

    <view class="form-card">
      <!-- 标题 -->
      <view class="form-item">
        <view class="form-label">通知标题</view>
        <input class="input-box" type="text" :placeholder="placeholder.title" />
      </view>

      <!-- 接收对象：年级 or 班级 -->
      <view class="form-item">
        <view class="form-label">接收对象</view>
        <view class="select-box">
          <view class="select-tag" v-for="item in targetList" :key="item">{{ item }}</view>
        </view>
      </view>

      <!-- 内容 -->
      <view class="form-item">
        <view class="form-label">通知内容</view>
        <textarea class="input-box textarea" :placeholder="placeholder.content"></textarea>
      </view>

      <!-- 发布按钮 -->
      <view class="btn-submit">发布通知</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 'grade',
      placeholder: {
        title: '请输入通知标题',
        content: '请输入通知详细内容'
      },
      gradeList: ['高一', '高二', '高三'],
      classList: ['高一(1)班', '高一(2)班', '高二(1)班', '高二(2)班', '高三(1)班'],
      targetList: ['高一', '高二']
    };
  }
};
</script>