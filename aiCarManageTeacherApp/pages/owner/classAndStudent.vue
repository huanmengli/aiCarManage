<!-- 班级学生子页面 -->
<template>
  <view class="student-list-page">
    <view class="header-bar">
      <view class="back-btn" @click="handleBack">
        <text class="arrow">‹</text>
      </view>
      <text class="title">{{ className }}</text>
      <view class="placeholder"></view>
    </view>

    <view class="student-count">
      <text class="count-value">{{ students.length }}</text>
      <text class="count-label">名学生</text>
    </view>

    <view class="student-list">
      <view
          v-for="student in students"
          :key="student.id"
          class="student-card"
          @click="handleStudentClick(student)"
      >
        <view class="student-avatar">
          <image :src="student.avatar" mode="aspectFill"></image>
        </view>
        <view class="student-info">
          <text class="student-name">{{ student.name }}</text>
          <text class="contact-count">{{ student.contacts.length }} 位接送人</text>
        </view>
        <view class="student-arrow">›</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      className: '',
      students: []
    }
  },
  onLoad(options) {
    if (options.className) {
      this.className = decodeURIComponent(options.className)
    }
    if (options.students) {
      this.students = JSON.parse(decodeURIComponent(options.students))
    }
  },
  methods: {
    handleBack() {
      uni.navigateBack()
    },
    handleStudentClick(student) {
      uni.navigateTo({
        url: `/pages/setting/contact-list?studentName=${encodeURIComponent(student.name)}&contacts=${encodeURIComponent(JSON.stringify(student.contacts))}`
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.student-list-page {
  min-height: 100vh;
  background: #f8fafc;
}

.header-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 24rpx 32rpx;
  background: white;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.back-btn {
  width: 64rpx;
  height: 64rpx;
  background: #f5f7fa;
  border-radius: 16rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.arrow {
  font-size: 48rpx;
  color: #333;
  font-weight: bold;
}

.title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1e293b;
}

.placeholder {
  width: 64rpx;
}

.student-count {
  display: flex;
  align-items: baseline;
  gap: 16rpx;
  padding: 32rpx;
}

.count-value {
  font-size: 72rpx;
  font-weight: 700;
  color: #2563eb;
}

.count-label {
  font-size: 32rpx;
  color: #64748b;
}

.student-list {
  padding: 0 32rpx;
}

.student-card {
  display: flex;
  align-items: center;
  background: white;
  border-radius: 24rpx;
  padding: 24rpx;
  margin-bottom: 24rpx;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.student-avatar {
  width: 96rpx;
  height: 96rpx;
  border-radius: 50%;
  overflow: hidden;
  margin-right: 24rpx;
  border: 4rpx solid #e2e8f0;
}

.student-avatar image {
  width: 100%;
  height: 100%;
}

.student-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.student-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
}

.contact-count {
  font-size: 26rpx;
  color: #64748b;
}

.student-arrow {
  font-size: 48rpx;
  color: #94a3b8;
}
</style>
