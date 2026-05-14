<!-- pages/notice/notice-list.vue -->
<<template>
  <view class="container">
    <!-- 顶部导航栏 -->
    <view class="nav-bar">
      <view class="nav-back" @click="goBack">
        <text class="icon">←</text>
      </view>
      <text class="nav-title">家长通知动态</text>
      <view class="nav-placeholder"></view>
    </view>

    <!-- 统计卡片 -->
    <view class="stats-header">
      <view class="stats-info">
        <text class="stats-count">共 {{ noticeList.length }} 位家长</text>
        <text class="stats-detail">已收到 {{ receivedCount }} 人 · 未收到 {{ unreceivedCount }} 人</text>
      </view>
      <view class="stats-icon">
        <text class="icon-bell">🔔</text>
      </view>
    </view>

    <!-- 筛选标签 -->
    <view class="filter-tabs">
      <view 
        class="tab-item" 
        :class="{ active: currentTab === 'all' }"
        @click="switchTab('all')"
      >
        全部
        <text class="tab-badge">{{ noticeList.length }}</text>
      </view>
      <view 
        class="tab-item" 
        :class="{ active: currentTab === 'received' }"
        @click="switchTab('received')"
      >
        已收到
        <text class="tab-badge">{{ receivedCount }}</text>
      </view>
      <view 
        class="tab-item" 
        :class="{ active: currentTab === 'unreceived' }"
        @click="switchTab('unreceived')"
      >
        未收到
        <text class="tab-badge">{{ unreceivedCount }}</text>
      </view>
    </view>

    <!-- 家长通知列表 -->
    <scroll-view class="notice-list" scroll-y>
      <view 
        class="notice-card" 
        v-for="(item, index) in filteredList" 
        :key="index"
      >
        <image class="avatar" :src="item.avatar" mode="aspectFill"></image>
        <view class="notice-info">
          <view class="info-row">
            <text class="student-name">{{ item.studentName }}</text>
            <text class="class-name">{{ item.className }}</text>
          </view>
          <view class="parent-row">
            <text class="parent-label">家长：</text>
            <text class="parent-name">{{ item.parentName }}</text>
          </view>
        </view>
        <view class="status-wrapper">
          <text class="status-tag" :class="item.status">{{ item.statusText }}</text>
          <text class="notice-time">{{ item.time }}</text>
        </view>
      </view>

      <!-- 空状态 -->
      <view class="empty-state" v-if="filteredList.length === 0">
        <text class="empty-icon">📭</text>
        <text class="empty-text">暂无相关通知记录</text>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      currentTab: 'all',
      
      noticeList: [
        {
          studentName: '张晓明',
          className: '三年级2班',
          parentName: '张大民',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=zhang',
          status: 'received',
          statusText: '已收到',
          time: '16:32'
        },
        {
          studentName: '李小红',
          className: '三年级2班',
          parentName: '李卫国',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=li',
          status: 'unreceived',
          statusText: '未收到',
          time: '16:30'
        },
        {
          studentName: '王小虎',
          className: '三年级2班',
          parentName: '王建军',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=wang',
          status: 'received',
          statusText: '已收到',
          time: '16:28'
        },
        {
          studentName: '刘小美',
          className: '三年级2班',
          parentName: '刘志强',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=liu',
          status: 'received',
          statusText: '已收到',
          time: '16:25'
        },
        {
          studentName: '陈大伟',
          className: '三年级2班',
          parentName: '陈国华',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=chen',
          status: 'unreceived',
          statusText: '未收到',
          time: '16:20'
        },
        {
          studentName: '赵小芳',
          className: '三年级2班',
          parentName: '赵明华',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=zhao',
          status: 'received',
          statusText: '已收到',
          time: '16:15'
        },
        {
          studentName: '孙小强',
          className: '三年级2班',
          parentName: '孙建国',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=sun',
          status: 'unreceived',
          statusText: '未收到',
          time: '16:10'
        },
        {
          studentName: '周小敏',
          className: '三年级2班',
          parentName: '周丽华',
          avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=zhou',
          status: 'received',
          statusText: '已收到',
          time: '16:05'
        }
      ]
    }
  },
  
  computed: {
    receivedCount() {
      return this.noticeList.filter(item => item.status === 'received').length
    },
    
    unreceivedCount() {
      return this.noticeList.filter(item => item.status === 'unreceived').length
    },
    
    filteredList() {
      if (this.currentTab === 'all') {
        return this.noticeList
      }
      return this.noticeList.filter(item => item.status === this.currentTab)
    }
  },
  
  methods: {
    goBack() {
      uni.navigateBack()
    },
    
    switchTab(tab) {
      this.currentTab = tab
    }
  }
}
</script>

<style>
page {
  background-color: #f5f7fa;
  height: 100%;
}

.container {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

/* 顶部导航栏 */
.nav-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 24rpx 32rpx;
  background: #fff;
}

.nav-back {
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-back .icon {
  font-size: 36rpx;
  color: #1f2937;
}

.nav-title {
  font-size: 34rpx;
  font-weight: 600;
  color: #1f2937;
}

.nav-placeholder {
  width: 60rpx;
}

/* 统计卡片 */
.stats-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 32rpx;
  margin: 24rpx 32rpx 0;
  background: #fff;
  border-radius: 24rpx;
}

.stats-info {
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.stats-count {
  font-size: 32rpx;
  font-weight: 600;
  color: #1f2937;
}

.stats-detail {
  font-size: 24rpx;
  color: #9ca3af;
}

.stats-icon {
  width: 80rpx;
  height: 80rpx;
  background: #eff6ff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.stats-icon .icon-bell {
  font-size: 40rpx;
}

/* 筛选标签 */
.filter-tabs {
  display: flex;
  gap: 16rpx;
  padding: 24rpx 32rpx;
}

.tab-item {
  display: flex;
  align-items: center;
  gap: 8rpx;
  padding: 16rpx 32rpx;
  background: #fff;
  border-radius: 32rpx;
  font-size: 26rpx;
  color: #6b7280;
  border: 2rpx solid transparent;
  transition: all 0.3s;
}

.tab-item.active {
  background: #eff6ff;
  color: #2563eb;
  border-color: #2563eb;
}

.tab-badge {
  font-size: 22rpx;
  background: #f3f4f6;
  color: #6b7280;
  padding: 2rpx 12rpx;
  border-radius: 12rpx;
}

.tab-item.active .tab-badge {
  background: #dbeafe;
  color: #2563eb;
}

/* 家长通知列表 */
.notice-list {
  flex: 1;
  padding: 0 32rpx;
  overflow-y: auto;
}

.notice-card {
  display: flex;
  align-items: center;
  gap: 24rpx;
  background: #fff;
  border-radius: 24rpx;
  padding: 28rpx 32rpx;
  margin-bottom: 20rpx;
}

.avatar {
  width: 88rpx;
  height: 88rpx;
  border-radius: 50%;
  background: #f3f4f6;
  flex-shrink: 0;
}

.notice-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.info-row {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.student-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #1f2937;
}

.class-name {
  font-size: 22rpx;
  color: #9ca3af;
  background: #f3f4f6;
  padding: 4rpx 16rpx;
  border-radius: 12rpx;
}

.parent-row {
  display: flex;
  align-items: center;
}

.parent-label {
  font-size: 24rpx;
  color: #9ca3af;
}

.parent-name {
  font-size: 24rpx;
  color: #6b7280;
}

.status-wrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 8rpx;
}

.status-tag {
  font-size: 24rpx;
  font-weight: 600;
  padding: 6rpx 20rpx;
  border-radius: 12rpx;
}

.status-tag.received {
  color: #10b981;
  background: rgba(16, 185, 129, 0.1);
}

.status-tag.unreceived {
  color: #f59e0b;
  background: rgba(245, 158, 11, 0.1);
}

.notice-time {
  font-size: 22rpx;
  color: #9ca3af;
}

/* 空状态 */
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 120rpx 0;
  gap: 24rpx;
}

.empty-icon {
  font-size: 80rpx;
}

.empty-text {
  font-size: 28rpx;
  color: #9ca3af;
}
</style>