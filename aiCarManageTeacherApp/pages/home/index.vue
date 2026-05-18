<!-- 首页数据查看页面 -->
<template>
  <view class="index-page">
    <view class="header">
      <view class="date-text">{{ currentDate }}</view>
      <view class="greeting">
        <text class="greeting-text">王老师，工作顺利</text>
        <view class="action-btn" @click="handleAction">
          <view class="grid-icon">
            <view class="grid-cell"></view>
            <view class="grid-cell"></view>
            <view class="grid-cell"></view>
            <view class="grid-cell"></view>
          </view>
        </view>
      </view>
    </view>

    <view class="stats-container">
      <view class="stat-card">
        <view class="stat-label">全部通知</view>
        <view class="stat-value blue">{{ stats.notified }}</view>
      </view>
      <view class="stat-card">
        <view class="stat-label">已收到</view>
        <view class="stat-value green">{{ stats.received }}</view>
      </view>
      <view class="stat-card">
        <view class="stat-label">未收到</view>
        <view class="stat-value orange">{{ stats.pending }}</view>
      </view>
    </view>

    <view class="progress-section">
      <view class="progress-circle">
        <canvas type="2d" id="progressCanvas" class="canvas"></canvas>
        <view class="progress-text">
          <text class="progress-percent">{{ progress }}%</text>
          <text class="progress-label">总体进度</text>
        </view>
      </view>
      <view class="progress-info">
        <view class="progress-title">今日接送动态</view>
        <view class="progress-detail">
          <text class="detail-label">当前高峰期</text>
          <text class="detail-value">{{ peakTime }}</text>
        </view>
        <view class="progress-bar">
          <view class="progress-bar-fill" :style="{ width: progress + '%' }"></view>
        </view>
        <view class="progress-tip">
          <text>{{ tip }}</text>
        </view>
      </view>
    </view>

    <view class="notification-section">
      <view class="notification-header">
        <text class="notification-title">家长通知动态</text>
        <text class="see-all" @click="handleSeeAll">查看全部</text>
      </view>
      <view class="notification-list">
        <view
            v-for="item in notifications"
            :key="item.id"
            class="notification-card"
        >
          <view class="avatar">
            <image src="https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=teacher%20avatar&image_size=square" mode="aspectFill" class="avatar-img"></image>
          </view>
          <view class="notification-info">
            <text class="student-name">{{ item.studentName }} {{ item.className }}</text>
            <text class="parent-name">家长：{{ item.parentName }}</text>
          </view>
          <view :class="['status-tag', item.status]">
            <text>{{ item.statusText }}</text>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      currentDate: '2026年5月14日 星期四',
      stats: {
        notified: 128,
        received: 84,
        pending: 44
      },
      progress: 66,
      peakTime: '16:30 - 17:15',
      tip: '当前西门车流量较大，建议引导后续家长前往南门区域',
      notifications: [
        {
          id: 1,
          studentName: '张晓明',
          className: '三年级2班',
          parentName: '张大民',
          status: 'received',
          statusText: '已收到'
        },
        {
          id: 2,
          studentName: '李小红',
          className: '三年级2班',
          parentName: '李卫国',
          status: 'pending',
          statusText: '未收到'
        },
        {
          id: 3,
          studentName: '王小虎',
          className: '三年级2班',
          parentName: '王建军',
          status: 'received',
          statusText: '已收到'
        }
      ]
    }
  },
  onLoad() {
    this.initProgress()
  },
  methods: {
    initProgress() {
      setTimeout(() => {
        const query = uni.createSelectorQuery().in(this)
        query.select('#progressCanvas')
            .fields({ node: true, size: true })
            .exec((res) => {
              if (!res[0]) return

              const canvas = res[0].node
              const ctx = canvas.getContext('2d')

              const dpr = (uni.getWindowInfo && uni.getWindowInfo().pixelRatio) || 2
              canvas.width = 240 * dpr
              canvas.height = 240 * dpr
              ctx.scale(dpr, dpr)

              const size = 240
              const centerX = size / 2
              const centerY = size / 2
              const radius = 100

              ctx.lineWidth = 20
              ctx.strokeStyle = '#e2e8f0'
              ctx.beginPath()
              ctx.arc(centerX, centerY, radius, 0, 2 * Math.PI)
              ctx.stroke()

              const progressAngle = (this.progress / 100) * 2 * Math.PI
              ctx.strokeStyle = '#2563eb'
              ctx.lineCap = 'round'
              ctx.beginPath()
              ctx.arc(centerX, centerY, radius, -Math.PI / 2, -Math.PI / 2 + progressAngle)
              ctx.stroke()
            })
      }, 100)
    },
    handleAction() {
      uni.showToast({
        title: '功能开发中',
        icon: 'none'
      })
    },
    handleSeeAll() {
      uni.showToast({
        title: '查看全部通知',
        icon: 'none'
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.index-page {
  min-height: 100vh;
  background: #f8fafc;
  padding-bottom: 120rpx;
}

.header {
  background: #ffffff;
  padding: 60rpx 32rpx 40rpx;
}

.date-text {
  font-size: 28rpx;
  color: #64748b;
  margin-bottom: 12rpx;
}

.greeting {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.greeting-text {
  font-size: 52rpx;
  font-weight: 700;
  color: #1e293b;
}

.action-btn {
  width: 96rpx;
  height: 96rpx;
  background: linear-gradient(135deg, #2563eb 0%, #3b82f6 100%);
  border-radius: 24rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8rpx 24rpx rgba(37, 99, 235, 0.25);
}

.grid-icon {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 8rpx;
}

.grid-cell {
  width: 16rpx;
  height: 16rpx;
  background: white;
  border-radius: 4rpx;
}

.stats-container {
  display: flex;
  gap: 24rpx;
  padding: 32rpx;
}

.stat-card {
  flex: 1;
  background: white;
  border-radius: 40rpx;
  padding: 40rpx 20rpx;
  text-align: center;
  border: 2rpx solid #e2e8f0;
}

.stat-label {
  font-size: 26rpx;
  color: #64748b;
  margin-bottom: 16rpx;
}

.stat-value {
  font-size: 60rpx;
  font-weight: 700;
}

.stat-value.blue {
  color: #2563eb;
}

.stat-value.green {
  color: #16a34a;
}

.stat-value.orange {
  color: #ea580c;
}

.progress-section {
  margin: 0 32rpx 32rpx;
  background: white;
  border-radius: 40rpx;
  padding: 40rpx;
  display: flex;
  gap: 32rpx;
  border: 2rpx solid #e2e8f0;
}

.progress-circle {
  width: 240rpx;
  height: 240rpx;
  position: relative;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.canvas {
  width: 100%;
  height: 100%;
}

.progress-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}

.progress-percent {
  display: block;
  font-size: 48rpx;
  font-weight: 700;
  color: #1e293b;
}

.progress-label {
  display: block;
  font-size: 24rpx;
  color: #64748b;
}

.progress-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.progress-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 24rpx;
}

.progress-detail {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 20rpx;
  margin-bottom: 20rpx;
}

.detail-label {
  font-size: 28rpx;
  color: #64748b;
}

.detail-value {
  font-size: 28rpx;
  font-weight: 600;
  color: #1e293b;
}

.progress-bar {
  height: 12rpx;
  background: #e2e8f0;
  border-radius: 6rpx;
  margin-bottom: 20rpx;
  overflow: hidden;
}

.progress-bar-fill {
  height: 100%;
  background: linear-gradient(90deg, #2563eb 0%, #3b82f6 100%);
  border-radius: 6rpx;
}

.progress-tip {
  font-size: 26rpx;
  color: #64748b;
  line-height: 1.6;
}

.notification-section {
  padding: 0 32rpx;
}

.notification-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24rpx;
}

.notification-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1e293b;
}

.see-all {
  font-size: 28rpx;
  color: #2563eb;
}

.notification-list {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}

.notification-card {
  background: white;
  border-radius: 40rpx;
  padding: 32rpx;
  display: flex;
  align-items: center;
  gap: 24rpx;
  border: 2rpx solid #e2e8f0;
}

.avatar {
  width: 104rpx;
  height: 104rpx;
  border-radius: 50%;
  overflow: hidden;
  flex-shrink: 0;
}

.avatar-img {
  width: 100%;
  height: 100%;
}

.notification-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.student-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 8rpx;
}

.parent-name {
  font-size: 26rpx;
  color: #64748b;
}

.status-tag {
  padding: 12rpx 24rpx;
  border-radius: 40rpx;
  font-size: 24rpx;
  font-weight: 500;
}

.status-tag.received {
  background: #dcfce7;
  color: #16a34a;
}

.status-tag.pending {
  background: #fef3c7;
  color: #ea580c;
}
</style>
