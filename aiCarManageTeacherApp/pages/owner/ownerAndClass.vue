<!-- 班级管理子页面 -->
<template>
  <view class="class-list-page">
    <view class="header-bar">
      <view class="back-btn" @click="handleBack">
        <text class="arrow">‹</text>
      </view>
      <text class="title">班级管理</text>
      <view class="placeholder"></view>
    </view>

    <view class="class-stats">
      <view class="stat-item">
        <text class="stat-value">{{ classList.length }}</text>
        <text class="stat-label">班级总数</text>
      </view>
      <view class="stat-item">
        <text class="stat-value">{{ totalStudents }}</text>
        <text class="stat-label">学生总数</text>
      </view>
    </view>

    <view class="class-list">
      <view
          v-for="cls in classList"
          :key="cls.id"
          class="class-card"
          @click="handleClassClick(cls)"
      >
        <view class="class-icon">
          <text class="icon">👥</text>
        </view>
        <view class="class-info">
          <text class="class-name">{{ cls.name }}</text>
          <text class="class-count">{{ cls.students.length }} 名学生</text>
        </view>
        <view class="class-arrow">›</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      classList: [
        {
          id: 1,
          name: '三年级(2)班',
          students: [
            {
              id: 101,
              name: '张晓明',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 1, name: '王美兰', relation: '奶奶', phone: '139 1234 5678', verified: true },
                { id: 2, name: '李国强', relation: '爷爷', phone: '136 8765 4321', verified: true }
              ]
            },
            {
              id: 102,
              name: '李小红',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 3, name: '张阿姨', relation: '保姆', phone: '158 5555 6666', verified: false }
              ]
            },
            {
              id: 103,
              name: '王小虎',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 4, name: '王建军', relation: '父亲', phone: '138 1111 2222', verified: true },
                { id: 5, name: '刘芳', relation: '母亲', phone: '139 3333 4444', verified: true },
                { id: 6, name: '王奶奶', relation: '奶奶', phone: '137 5555 6666', verified: true }
              ]
            }
          ]
        },
        {
          id: 2,
          name: '四年级(1)班',
          students: [
            {
              id: 201,
              name: '赵小明',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 7, name: '赵大海', relation: '父亲', phone: '135 7777 8888', verified: true }
              ]
            },
            {
              id: 202,
              name: '孙小丽',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 8, name: '孙建国', relation: '父亲', phone: '136 9999 0000', verified: true },
                { id: 9, name: '周老师', relation: '姑姑', phone: '138 2222 3333', verified: false }
              ]
            }
          ]
        },
        {
          id: 3,
          name: '五年级(3)班',
          students: [
            {
              id: 301,
              name: '陈小强',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 10, name: '陈大山', relation: '父亲', phone: '139 4444 5555', verified: true }
              ]
            },
            {
              id: 302,
              name: '周小燕',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 11, name: '周伟', relation: '父亲', phone: '137 6666 7777', verified: true },
                { id: 12, name: '吴梅', relation: '母亲', phone: '138 8888 9999', verified: true }
              ]
            },
            {
              id: 303,
              name: '吴小涛',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 13, name: '吴刚', relation: '父亲', phone: '135 1111 2222', verified: true }
              ]
            },
            {
              id: 304,
              name: '郑小芳',
              avatar: 'https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop',
              contacts: [
                { id: 14, name: '郑华', relation: '父亲', phone: '136 3333 4444', verified: true },
                { id: 15, name: '郑奶奶', relation: '奶奶', phone: '137 5555 6666', verified: false }
              ]
            }
          ]
        }
      ]
    }
  },
  computed: {
    totalStudents() {
      return this.classList.reduce((sum, cls) => sum + cls.students.length, 0)
    }
  },
  methods: {
    handleBack() {
      uni.navigateBack()
    },
    handleClassClick(cls) {
      uni.navigateTo({
        url: `/pages/setting/student-list?className=${encodeURIComponent(cls.name)}&students=${encodeURIComponent(JSON.stringify(cls.students))}`
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.class-list-page {
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

.class-stats {
  display: flex;
  gap: 24rpx;
  padding: 32rpx;
}

.stat-item {
  flex: 1;
  background: white;
  border-radius: 24rpx;
  padding: 32rpx;
  text-align: center;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.stat-value {
  display: block;
  font-size: 56rpx;
  font-weight: 700;
  color: #2563eb;
  margin-bottom: 8rpx;
}

.stat-label {
  display: block;
  font-size: 26rpx;
  color: #64748b;
}

.class-list {
  padding: 0 32rpx;
}

.class-card {
  display: flex;
  align-items: center;
  background: white;
  border-radius: 24rpx;
  padding: 32rpx;
  margin-bottom: 24rpx;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.class-icon {
  width: 96rpx;
  height: 96rpx;
  background: #dbeafe;
  border-radius: 24rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 24rpx;
}

.icon {
  font-size: 48rpx;
}

.class-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.class-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
}

.class-count {
  font-size: 26rpx;
  color: #64748b;
}

.class-arrow {
  font-size: 48rpx;
  color: #94a3b8;
}
</style>
