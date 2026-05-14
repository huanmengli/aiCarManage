<!-- pages/map/map.vue -->
<<template>
  <view class="container">
    <!-- 搜索栏 -->
    <view class="search-bar">
      <view class="search-box">
        <text class="icon-search">🔍</text>
        <input 
          type="text" 
          v-model="searchValue" 
          placeholder="搜索学校或地点"
        />
      </view>
    </view>

    <!-- 地图区域 -->
    <view class="map-wrapper">
      <map
        id="schoolMap"
        class="school-map"
        :longitude="school.longitude"
        :latitude="school.latitude"
        :scale="scale"
        :markers="markers"
        :polyline="polyline"
        show-location
      ></map>
      
      <!-- 学校标签 -->
      <cover-view class="school-label-wrapper">
        <cover-view class="school-label">学校</cover-view>
        <cover-view class="school-label-arrow"></cover-view>
      </cover-view>
    </view>

    <!-- 路线推荐列表 -->
    <scroll-view class="routes-section" scroll-y>
      <view class="section-title">推荐接送路线</view>

      <view 
        class="route-card" 
        :class="{ active: currentRoute === index }"
        v-for="(item, index) in routes" 
        :key="index"
        @click="selectRoute(index)"
      >
        <view class="route-header">
          <view class="route-name">
            <view class="route-dot" :class="item.statusClass"></view>
            <text>{{ item.name }}</text>
          </view>
          <view class="route-status" :class="item.statusClass">{{ item.statusText }}</view>
        </view>
        
        <view class="route-info">
          <view class="info-item">
            <text class="icon">⏱</text>
            <text>{{ item.time }} 分钟</text>
          </view>
          <view class="info-item">
            <text class="icon">📍</text>
            <text>{{ item.distance }} km</text>
          </view>
        </view>
        
        <view v-if="item.recommend" class="recommend-tag">推荐</view>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      searchValue: '第一实验小学 (西门)',
      scale: 15,
      
      school: {
        latitude: 39.9042,
        longitude: 116.4074
      },
      
      markers: [{
        id: 1,
        latitude: 39.9042,
        longitude: 116.4074,
        title: '第一实验小学'
      }],
      
      currentRoute: 0,
      
      routes: [
        {
          name: '朝阳路 → 实验路',
          statusText: '畅通',
          statusClass: 'green',
          time: 8,
          distance: 2.4,
          recommend: true,
          points: [
            { latitude: 39.9080, longitude: 116.4000 },
            { latitude: 39.9080, longitude: 116.4040 },
            { latitude: 39.9060, longitude: 116.4040 },
            { latitude: 39.9042, longitude: 116.4074 }
          ]
        },
        {
          name: '环城东路 → 育才街',
          statusText: '缓行',
          statusClass: 'yellow',
          time: 15,
          distance: 3.1,
          recommend: false,
          points: [
            { latitude: 39.9042, longitude: 116.4150 },
            { latitude: 39.9042, longitude: 116.4120 },
            { latitude: 39.9042, longitude: 116.4100 },
            { latitude: 39.9042, longitude: 116.4074 }
          ]
        },
        {
          name: '建设大街 → 学校南路',
          statusText: '拥堵',
          statusClass: 'red',
          time: 24,
          distance: 1.8,
          recommend: false,
          points: [
            { latitude: 39.9000, longitude: 116.4074 },
            { latitude: 39.9020, longitude: 116.4074 },
            { latitude: 39.9030, longitude: 116.4074 },
            { latitude: 39.9042, longitude: 116.4074 }
          ]
        }
      ],
      
      polyline: []
    }
  },
  
  onLoad() {
    this.initPolyline()
  },
  
  methods: {
    initPolyline() {
      const colors = ['#10b981', '#f59e0b', '#ef4444']
      
      this.polyline = this.routes.map((route, index) => ({
        points: route.points,
        color: colors[index],
        width: index === this.currentRoute ? 6 : 3,
        dottedLine: index !== 0,
        zIndex: index === this.currentRoute ? 100 : 1
      }))
    },
    
    selectRoute(index) {
      this.currentRoute = index
      this.initPolyline()
    }
  }
}
</script>

<style>
page {
  background-color: #f5f6fa;
  height: 100%;
}

.container {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

/* 搜索栏 */
.search-bar {
  background: #fff;
  padding: 24rpx 32rpx;
  box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.05);
  z-index: 100;
}

.search-box {
  display: flex;
  align-items: center;
  background: #f0f2f5;
  border-radius: 48rpx;
  padding: 20rpx 32rpx;
}

.icon-search {
  font-size: 28rpx;
  margin-right: 16rpx;
}

.search-box input {
  flex: 1;
  font-size: 30rpx;
  color: #333;
}

/* 地图区域 */
.map-wrapper {
  position: relative;
  height: 640rpx;
  background: #e8ecf1;
  overflow: hidden;
}

.school-map {
  width: 100%;
  height: 100%;
}

.school-label-wrapper {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  pointer-events: none;
}

.school-label {
  background: #2563eb;
  color: white;
  padding: 12rpx 24rpx;
  border-radius: 16rpx;
  font-size: 28rpx;
  font-weight: 600;
  box-shadow: 0 4rpx 16rpx rgba(37, 99, 235, 0.4);
}

.school-label-arrow {
  width: 0;
  height: 0;
  border-left: 12rpx solid transparent;
  border-right: 12rpx solid transparent;
  border-top: 12rpx solid #2563eb;
  margin-top: -2rpx;
}

/* 路线列表 */
.routes-section {
  flex: 1;
  padding: 32rpx;
  overflow-y: auto;
}

.section-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #1f2937;
  margin-bottom: 24rpx;
}

.route-card {
  background: #fff;
  border-radius: 32rpx;
  padding: 32rpx;
  margin-bottom: 24rpx;
  box-shadow: 0 2rpx 6rpx rgba(0,0,0,0.05);
  border: 4rpx solid transparent;
}

.route-card.active {
  border-color: #2563eb;
  background: #eff6ff;
}

.route-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16rpx;
}

.route-name {
  display: flex;
  align-items: center;
  gap: 16rpx;
  font-size: 30rpx;
  font-weight: 600;
  color: #1f2937;
}

.route-dot {
  width: 16rpx;
  height: 16rpx;
  border-radius: 50%;
  flex-shrink: 0;
}

.route-dot.green {
  background: #10b981;
  box-shadow: 0 0 0 6rpx rgba(16, 185, 129, 0.2);
}

.route-dot.yellow {
  background: #f59e0b;
  box-shadow: 0 0 0 6rpx rgba(245, 158, 11, 0.2);
}

.route-dot.red {
  background: #ef4444;
  box-shadow: 0 0 0 6rpx rgba(239, 68, 68, 0.2);
}

.route-status {
  font-size: 26rpx;
  font-weight: 600;
  padding: 8rpx 20rpx;
  border-radius: 24rpx;
}

.route-status.green {
  color: #10b981;
  background: rgba(16, 185, 129, 0.1);
}

.route-status.yellow {
  color: #f59e0b;
  background: rgba(245, 158, 11, 0.1);
}

.route-status.red {
  color: #ef4444;
  background: rgba(239, 68, 68, 0.1);
}

.route-info {
  display: flex;
  gap: 32rpx;
  margin-top: 16rpx;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 8rpx;
  color: #6b7280;
  font-size: 26rpx;
}

.info-item .icon {
  font-size: 24rpx;
}

.recommend-tag {
  display: inline-block;
  margin-top: 16rpx;
  color: #2563eb;
  font-size: 26rpx;
  font-weight: 600;
}
</style>