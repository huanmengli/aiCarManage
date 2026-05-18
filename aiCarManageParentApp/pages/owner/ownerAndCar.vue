<!-- pages/vehicle/vehicle.vue -->
<<template>
  <view class="container">
    <!-- 顶部导航栏 -->
    <view class="nav-bar">
      <view class="nav-back" @click="goBack">
        <text class="icon">←</text>
      </view>
      <text class="nav-title">车辆管理</text>
      <view class="nav-placeholder"></view>
    </view>

    <!-- 车辆统计 -->
    <view class="vehicle-header">
      <view class="header-info">
        <text class="vehicle-count">共 {{ vehicleList.length }} 辆车</text>
        <text class="vehicle-limit">最多可添加5辆车</text>
      </view>
      <view class="header-icon">
        <text class="icon-car">🚗</text>
      </view>
    </view>

    <!-- 车辆列表 -->
    <scroll-view class="vehicle-list" scroll-y>
      <view 
        class="vehicle-card" 
        v-for="(item, index) in vehicleList" 
        :key="index"
      >
        <view class="card-left">
          <view class="car-icon-wrapper">
            <text class="car-icon">🚙</text>
          </view>
          <view class="car-info">
            <view class="plate-row">
              <text class="plate-number">{{ item.plate }}</text>
              <text v-if="item.isDefault" class="default-tag">默认</text>
            </view>
            <text class="car-detail">{{ item.color }} · {{ item.brand }}</text>
          </view>
        </view>
        <view class="card-right">
          <view class="btn-edit" @click="openEdit(item, index)">
            <text class="icon">✏️</text>
          </view>
          <view class="btn-delete" @click="deleteVehicle(index)">
            <text class="icon">🗑️</text>
          </view>
        </view>
      </view>
    </scroll-view>

    <!-- 添加按钮 -->
    <view class="add-btn-wrapper">
      <view class="add-btn" @click="openAdd">
        <text class="icon">+</text>
        <text>添加车辆</text>
      </view>
    </view>

    <!-- 编辑/添加弹窗 -->
    <view class="modal-mask" v-if="showModal" @click="closeModal"></view>
    <view class="modal-content" v-if="showModal">
      <view class="modal-header">
        <text class="modal-title">{{ isEdit ? '编辑车辆' : '添加车辆' }}</text>
        <view class="modal-close" @click="closeModal">
          <text>✕</text>
        </view>
      </view>

      <view class="modal-body">
        <!-- 车牌号 -->
        <view class="form-item">
          <text class="form-label">车牌号</text>
          <input 
            class="form-input" 
            v-model="form.plate" 
            placeholder="请输入车牌号"
            maxlength="10"
          />
        </view>

        <!-- 车辆颜色 -->
        <view class="form-item">
          <text class="form-label">车辆颜色</text>
          <view class="color-list">
            <view 
              class="color-item" 
              v-for="(color, idx) in colorList" 
              :key="idx"
              :class="{ active: form.color === color.name }"
              @click="selectColor(color)"
            >
              <view class="color-circle" :style="{ backgroundColor: color.value }"></view>
            </view>
          </view>
        </view>

        <!-- 品牌型号 -->
        <view class="form-item">
          <text class="form-label">品牌型号</text>
          <input 
            class="form-input" 
            v-model="form.brand" 
            placeholder="请输入品牌型号"
          />
        </view>

        <!-- 设为默认 -->
        <view class="form-item switch-item">
          <text class="form-label">设为默认车辆</text>
          <switch 
            :checked="form.isDefault" 
            color="#2563eb" 
            @change="toggleDefault"
          />
        </view>
      </view>

      <view class="modal-footer">
        <view class="btn-cancel" @click="closeModal">取消</view>
        <view class="btn-confirm" @click="confirm">确认</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      vehicleList: [
        {
          plate: '粤B·88888',
          color: '黑色',
          brand: '特斯拉 Model 3',
          isDefault: true
        },
        {
          plate: '粤B·66666',
          color: '白色',
          brand: '比亚迪 汉 EV',
          isDefault: false
        },
        {
          plate: '粤B·99999',
          color: '灰色',
          brand: '蔚来 ET5',
          isDefault: false
        }
      ],
      
      colorList: [
        { name: '白色', value: '#ffffff', border: true },
        { name: '黑色', value: '#1f2937' },
        { name: '灰色', value: '#9ca3af' },
        { name: '银色', value: '#d1d5db' },
        { name: '红色', value: '#ef4444' },
        { name: '蓝色', value: '#3b82f6' }
      ],
      
      showModal: false,
      isEdit: false,
      editIndex: -1,
      
      form: {
        plate: '',
        color: '黑色',
        brand: '',
        isDefault: false
      }
    }
  },
  
  methods: {
    goBack() {
      uni.navigateBack()
    },
    
    openAdd() {
      if (this.vehicleList.length >= 5) {
        uni.showToast({
          title: '最多添加5辆车',
          icon: 'none'
        })
        return
      }
      
      this.isEdit = false
      this.editIndex = -1
      this.form = {
        plate: '',
        color: '黑色',
        brand: '',
        isDefault: false
      }
      this.showModal = true
    },
    
    openEdit(item, index) {
      this.isEdit = true
      this.editIndex = index
      this.form = {
        plate: item.plate,
        color: item.color,
        brand: item.brand,
        isDefault: item.isDefault
      }
      this.showModal = true
    },
    
    closeModal() {
      this.showModal = false
    },
    
    selectColor(color) {
      this.form.color = color.name
    },
    
    toggleDefault(e) {
      this.form.isDefault = e.detail.value
    },
    
    confirm() {
      if (!this.form.plate.trim()) {
        uni.showToast({
          title: '请输入车牌号',
          icon: 'none'
        })
        return
      }
      
      if (!this.form.brand.trim()) {
        uni.showToast({
          title: '请输入品牌型号',
          icon: 'none'
        })
        return
      }
      
      // 如果设为默认，取消其他默认
      if (this.form.isDefault) {
        this.vehicleList.forEach(item => {
          item.isDefault = false
        })
      }
      
      if (this.isEdit) {
        // 编辑
        this.vehicleList[this.editIndex] = {
          plate: this.form.plate,
          color: this.form.color,
          brand: this.form.brand,
          isDefault: this.form.isDefault
        }
      } else {
        // 新增
        this.vehicleList.push({
          plate: this.form.plate,
          color: this.form.color,
          brand: this.form.brand,
          isDefault: this.form.isDefault
        })
      }
      
      this.showModal = false
    },
    
    deleteVehicle(index) {
      uni.showModal({
        title: '提示',
        content: '确定删除该车辆？',
        success: (res) => {
          if (res.confirm) {
            this.vehicleList.splice(index, 1)
          }
        }
      })
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

/* 车辆统计 */
.vehicle-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 32rpx;
  margin: 24rpx 32rpx 0;
  background: #fff;
  border-radius: 24rpx;
}

.header-info {
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.vehicle-count {
  font-size: 32rpx;
  font-weight: 600;
  color: #1f2937;
}

.vehicle-limit {
  font-size: 24rpx;
  color: #9ca3af;
}

.header-icon {
  width: 80rpx;
  height: 80rpx;
  background: #eff6ff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.header-icon .icon-car {
  font-size: 40rpx;
}

/* 车辆列表 */
.vehicle-list {
  flex: 1;
  padding: 24rpx 32rpx;
  overflow-y: auto;
}

.vehicle-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #fff;
  border-radius: 24rpx;
  padding: 32rpx;
  margin-bottom: 24rpx;
}

.card-left {
  display: flex;
  align-items: center;
  gap: 24rpx;
}

.car-icon-wrapper {
  width: 88rpx;
  height: 88rpx;
  background: #eff6ff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.car-icon {
  font-size: 44rpx;
}

.car-info {
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.plate-row {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.plate-number {
  font-size: 32rpx;
  font-weight: 600;
  color: #1f2937;
}

.default-tag {
  font-size: 22rpx;
  color: #2563eb;
  background: #eff6ff;
  padding: 4rpx 16rpx;
  border-radius: 12rpx;
}

.car-detail {
  font-size: 26rpx;
  color: #6b7280;
}

.card-right {
  display: flex;
  gap: 24rpx;
}

.btn-edit,
.btn-delete {
  width: 64rpx;
  height: 64rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
}

.btn-edit {
  background: #eff6ff;
}

.btn-edit .icon {
  font-size: 28rpx;
}

.btn-delete {
  background: #fef2f2;
}

.btn-delete .icon {
  font-size: 28rpx;
}

/* 添加按钮 */
.add-btn-wrapper {
  padding: 24rpx 32rpx;
  padding-bottom: calc(24rpx + env(safe-area-inset-bottom));
  background: #fff;
}

.add-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12rpx;
  background: #2563eb;
  color: #fff;
  font-size: 30rpx;
  font-weight: 600;
  padding: 28rpx 0;
  border-radius: 48rpx;
}

.add-btn .icon {
  font-size: 32rpx;
}

/* 弹窗遮罩 */
.modal-mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 998;
}

/* 弹窗内容 */
.modal-content {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #fff;
  border-radius: 48rpx 48rpx 0 0;
  padding: 40rpx 32rpx;
  padding-bottom: calc(40rpx + env(safe-area-inset-bottom));
  z-index: 999;
  animation: slideUp 0.3s ease;
}

@keyframes slideUp {
  from {
    transform: translateY(100%);
  }
  to {
    transform: translateY(0);
  }
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 40rpx;
}

.modal-title {
  font-size: 34rpx;
  font-weight: 600;
  color: #1f2937;
}

.modal-close {
  width: 56rpx;
  height: 56rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #9ca3af;
  font-size: 32rpx;
}

.modal-body {
  display: flex;
  flex-direction: column;
  gap: 32rpx;
}

.form-item {
  display: flex;
  flex-direction: column;
  gap: 16rpx;
}

.form-label {
  font-size: 28rpx;
  color: #6b7280;
}

.form-input {
  background: #f5f7fa;
  border-radius: 16rpx;
  padding: 24rpx 32rpx;
  font-size: 30rpx;
  color: #1f2937;
}

.color-list {
  display: flex;
  gap: 24rpx;
}

.color-item {
  width: 64rpx;
  height: 64rpx;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4rpx;
  border: 4rpx solid transparent;
}

.color-item.active {
  border-color: #2563eb;
}

.color-circle {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  border: 2rpx solid #e5e7eb;
}

.switch-item {
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.modal-footer {
  display: flex;
  gap: 24rpx;
  margin-top: 48rpx;
}

.btn-cancel,
.btn-confirm {
  flex: 1;
  text-align: center;
  padding: 28rpx 0;
  border-radius: 48rpx;
  font-size: 30rpx;
  font-weight: 600;
}

.btn-cancel {
  background: #f5f7fa;
  color: #6b7280;
}

.btn-confirm {
  background: #2563eb;
  color: #fff;
}
</style>