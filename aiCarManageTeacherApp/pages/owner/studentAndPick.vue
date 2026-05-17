<!-- 学生接送人页面 -->
<template>
  <view class="contact-list-page">
    <view class="header-bar">
      <view class="back-btn" @click="handleBack">
        <text class="arrow">‹</text>
      </view>
      <text class="title">接送人管理</text>
      <view class="placeholder"></view>
    </view>

    <view class="contact-summary">
      <view class="summary-text">
        <text class="summary-count">共 {{ contacts.length }} 位接送人</text>
        <text class="summary-hint">可添加多位家庭成员协助接送</text>
      </view>
      <view class="summary-icon">
        <text class="icon">👥</text>
      </view>
    </view>

    <view class="contact-list">
      <view
          v-for="contact in contacts"
          :key="contact.id"
          class="contact-card"
      >
        <view class="contact-avatar">
          <text class="avatar-text">{{ contact.name.charAt(0) }}</text>
        </view>
        <view class="contact-info">
          <view class="contact-name-row">
            <text class="contact-name">{{ contact.name }}</text>
            <text class="contact-relation">{{ contact.relation }}</text>
            <text :class="['contact-status', contact.verified ? 'verified' : 'unverified']">
              {{ contact.verified ? '已认证' : '未认证' }}
            </text>
          </view>
          <text class="contact-phone">{{ contact.phone }}</text>
        </view>
        <view class="contact-actions">
          <view class="action-btn edit" @click="handleEdit(contact)">
            <text class="action-icon">✏️</text>
          </view>
          <view class="action-btn delete" @click="handleDelete(contact)">
            <text class="action-icon">🗑️</text>
          </view>
        </view>
      </view>
    </view>

    <view class="add-contact-section">
      <button class="add-contact-btn" @click="handleAdd">
        <text class="plus-icon">+</text>
        <text>添加接送人</text>
      </button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      studentName: '',
      contacts: []
    }
  },
  onLoad(options) {
    if (options.studentName) {
      this.studentName = decodeURIComponent(options.studentName)
    }
    if (options.contacts) {
      this.contacts = JSON.parse(decodeURIComponent(options.contacts))
    }
  },
  methods: {
    handleBack() {
      uni.navigateBack()
    },
    handleEdit(contact) {
      uni.showToast({
        title: `编辑 ${contact.name}`,
        icon: 'none'
      })
    },
    handleDelete(contact) {
      uni.showModal({
        title: '确认删除',
        content: `确定要删除接送人 ${contact.name} 吗？`,
        success: (res) => {
          if (res.confirm) {
            uni.showToast({
              title: '已删除',
              icon: 'success'
            })
          }
        }
      })
    },
    handleAdd() {
      uni.showToast({
        title: '添加接送人',
        icon: 'none'
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.contact-list-page {
  min-height: 100vh;
  background: #f8fafc;
  padding-bottom: 160rpx;
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

.contact-summary {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  margin: 32rpx;
  padding: 32rpx;
  border-radius: 24rpx;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.summary-text {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.summary-count {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
}

.summary-hint {
  font-size: 26rpx;
  color: #64748b;
}

.summary-icon {
  width: 96rpx;
  height: 96rpx;
  background: #dbeafe;
  border-radius: 24rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon {
  font-size: 48rpx;
}

.contact-list {
  padding: 0 32rpx;
}

.contact-card {
  display: flex;
  align-items: center;
  background: white;
  border-radius: 24rpx;
  padding: 24rpx;
  margin-bottom: 24rpx;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.contact-avatar {
  width: 96rpx;
  height: 96rpx;
  border-radius: 50%;
  background: #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 24rpx;
}

.avatar-text {
  font-size: 40rpx;
  font-weight: 600;
  color: #64748b;
}

.contact-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.contact-name-row {
  display: flex;
  align-items: center;
  gap: 16rpx;
  flex-wrap: wrap;
}

.contact-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
}

.contact-relation {
  font-size: 26rpx;
  color: #64748b;
  padding: 4rpx 16rpx;
  background: #f1f5f9;
  border-radius: 16rpx;
}

.contact-status {
  font-size: 24rpx;
  font-weight: 500;
  padding: 4rpx 16rpx;
  border-radius: 16rpx;

  &.verified {
    background: #dcfce7;
    color: #16a34a;
  }

  &.unverified {
    background: #fef3c7;
    color: #ea580c;
  }
}

.contact-phone {
  font-size: 28rpx;
  color: #94a3b8;
}

.contact-actions {
  display: flex;
  gap: 16rpx;
}

.action-btn {
  width: 72rpx;
  height: 72rpx;
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;

  &.edit {
    background: #dbeafe;
  }

  &.delete {
    background: #fee2e2;
  }
}

.action-icon {
  font-size: 32rpx;
}

.add-contact-section {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 32rpx;
  background: white;
  box-shadow: 0 -8rpx 40rpx rgba(0, 0, 0, 0.08);
}

.add-contact-btn {
  width: 100%;
  padding: 36rpx;
  background: linear-gradient(135deg, #2563eb 0%, #3b82f6 100%);
  border: none;
  border-radius: 24rpx;
  color: white;
  font-size: 32rpx;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16rpx;
}

.plus-icon {
  font-size: 40rpx;
  font-weight: bold;
}
</style>
