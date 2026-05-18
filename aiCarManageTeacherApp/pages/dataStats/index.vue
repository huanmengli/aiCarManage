<!-- 数据统计页面 -->
<template>
  <view class="data-page">
    <view class="header">
      <text class="page-title">数据统计</text>
    </view>
    <view class="tab-bar">
      <view
          v-for="tab in tabs"
          :key="tab.id"
          :class="['tab-item', { active: currentTab === tab.id }]"
          @click="handleTabChange(tab.id)"
      >
        <text>{{ tab.name }}</text>
      </view>
    </view>

    <view class="content">
      <view class="core-stats-section">
        <view class="core-stats-card">
          <view class="core-stats-row">
            <view class="core-stat-item">
              <text class="core-stat-label">平均接送时长</text>
              <view class="core-stat-value">
                <text class="value">{{ currentStats.avgDuration }}</text>
                <text class="unit">min</text>
              </view>
              <view class="core-stat-trend" :class="currentStats.avgTrend">
                <text class="trend-icon">{{ currentStats.avgTrend === 'up' ? '↗' : '↘' }}</text>
                <text class="trend-text">{{ currentStats.avgTrendText }}</text>
              </view>
            </view>
            <view class="core-stat-item">
              <text class="core-stat-label">今日接送车辆</text>
              <view class="core-stat-value">
                <text class="value">{{ currentStats.todayVehicles }}</text>
                <text class="unit">辆</text>
              </view>
              <view class="core-stat-trend stable">
                <text class="trend-icon">↗</text>
                <text class="trend-text">运行平稳</text>
              </view>
            </view>
          </view>
        </view>
      </view>

      <view class="chart-section" v-if="currentTab !== 'day'">
        <view class="chart-card">
          <view class="chart-title">
            <text>{{ currentTab === 'week' ? '每周接送时间趋势 (MIN)' : '每月接送时间趋势 (MIN)' }}</text>
          </view>
          <view class="chart-container">
            <canvas type="2d" id="lineChart" class="line-canvas"></canvas>
          </view>
        </view>
      </view>

      <view class="grade-section">
        <view class="grade-card">
          <view class="grade-title">
            <text>各年级平均耗时对比</text>
          </view>
          <view class="grade-list">
            <view
                v-for="grade in gradeData"
                :key="grade.id"
                class="grade-item"
            >
              <view class="grade-info">
                <view class="grade-dot" :style="{ backgroundColor: grade.color }"></view>
                <text class="grade-name">{{ grade.name }}</text>
              </view>
              <view class="grade-bar-container">
                <view class="grade-bar-bg"></view>
                <view
                    class="grade-bar"
                    :style="{
                    width: (grade.duration / maxDuration * 100) + '%',
                    backgroundColor: grade.color
                  }"
                ></view>
              </view>
              <text class="grade-duration">{{ grade.duration }}m</text>
            </view>
          </view>
        </view>
      </view>

      <view class="hint-section" v-if="currentTab === 'day'">
        <view class="hint-card">
          <view class="hint-icon">📈</view>
          <view class="hint-text">
            <text class="hint-title">查看更多分析</text>
            <text class="hint-desc">切换到「本周」或「本月」查看趋势图表</text>
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
      currentTab: 'day',
      tabs: [
        { id: 'day', name: '本日' },
        { id: 'week', name: '本周' },
        { id: 'month', name: '本月' }
      ],
      dayStats: {
        avgDuration: '14.2',
        todayVehicles: '342',
        avgTrend: 'down',
        avgTrendText: '较昨日下降 8%'
      },
      weekStats: {
        avgDuration: '15.6',
        todayVehicles: '328',
        avgTrend: 'up',
        avgTrendText: '较上周上升 3%'
      },
      monthStats: {
        avgDuration: '14.8',
        todayVehicles: '335',
        avgTrend: 'down',
        avgTrendText: '较上月下降 2%'
      },
      weekChartData: [
        { label: '周一', value: 12 },
        { label: '周二', value: 18 },
        { label: '周三', value: 15 },
        { label: '周四', value: 14 },
        { label: '周五', value: 19 }
      ],
      monthChartData: [
        { label: '第1周', value: 15 },
        { label: '第2周', value: 18 },
        { label: '第3周', value: 14 },
        { label: '第4周', value: 17 }
      ],
      gradeData: [
        { id: 1, name: '一年级', duration: 18.5, color: '#2563eb' },
        { id: 2, name: '三年级', duration: 14.2, color: '#60a5fa' },
        { id: 3, name: '六年级', duration: 9.8, color: '#bfdbfe' }
      ]
    }
  },
  computed: {
    currentStats() {
      if (this.currentTab === 'week') {
        return this.weekStats
      } else if (this.currentTab === 'month') {
        return this.monthStats
      }
      return this.dayStats
    },
    maxDuration() {
      return Math.max(...this.gradeData.map(item => item.duration))
    }
  },
  onLoad() {
    this.initChart()
  },
  onShow() {
    if (this.currentTab !== 'day') {
      this.initChart()
    }
  },
  methods: {
    handleTabChange(tabId) {
      this.currentTab = tabId
      if (tabId !== 'day') {
        this.$nextTick(() => {
          this.initChart()
        })
      }
    },
    initChart() {
      const data = this.currentTab === 'week' ? this.weekChartData : this.monthChartData

      const query = uni.createSelectorQuery().in(this)
      query.select('#lineChart')
          .fields({ node: true, size: true })
          .exec((res) => {
            if (!res[0]) return

            const canvas = res[0].node
            const ctx = canvas.getContext('2d')

            const dpr = (uni.getWindowInfo && uni.getWindowInfo().pixelRatio) || 2
            canvas.width = 680 * dpr
            canvas.height = 480 * dpr
            ctx.scale(dpr, dpr)

            const paddingTop = 40
            const paddingBottom = 100
            const paddingLeft = 50
            const paddingRight = 30
            const chartWidth = 680 - paddingLeft - paddingRight
            const chartHeight = 480 - paddingTop - paddingBottom

            const maxValue = Math.max(...data.map(d => d.value))
            const minValue = Math.min(...data.map(d => d.value))
            const valuePadding = Math.max((maxValue - minValue) * 0.35, 4)
            const displayMax = maxValue + valuePadding
            const displayMin = Math.max(0, minValue - valuePadding)
            const valueRange = displayMax - displayMin

            ctx.fillStyle = '#ffffff'
            ctx.fillRect(0, 0, 680, 480)

            ctx.strokeStyle = '#f1f5f9'
            ctx.lineWidth = 1
            for (let i = 0; i <= 4; i++) {
              const y = paddingTop + (chartHeight / 4) * i
              ctx.beginPath()
              ctx.moveTo(paddingLeft, y)
              ctx.lineTo(680 - paddingRight, y)
              ctx.stroke()
            }

            ctx.fillStyle = '#94a3b8'
            ctx.font = '16px sans-serif'
            ctx.textAlign = 'right'
            ctx.textBaseline = 'middle'
            for (let i = 0; i <= 4; i++) {
              const y = paddingTop + (chartHeight / 4) * i
              const value = Math.round(displayMax - (valueRange / 4) * i)
              ctx.fillText(value.toString(), paddingLeft - 8, y)
            }

            const points = data.map((d, i) => {
              const x = paddingLeft + (chartWidth / (data.length - 1)) * i
              const normalizedValue = (d.value - displayMin) / valueRange
              const y = paddingTop + chartHeight - normalizedValue * chartHeight
              return { x, y, value: d.value, label: d.label }
            })

            const gradient = ctx.createLinearGradient(0, paddingTop, 0, paddingTop + chartHeight)
            gradient.addColorStop(0, 'rgba(37, 99, 235, 0.15)')
            gradient.addColorStop(1, 'rgba(37, 99, 235, 0.02)')
            ctx.fillStyle = gradient
            ctx.beginPath()
            ctx.moveTo(points[0].x, paddingTop + chartHeight)

            // 绘制填充区域的平滑曲线
            ctx.lineTo(points[0].x, points[0].y)

            for (let i = 0; i < points.length - 1; i++) {
              const p0 = i > 0 ? points[i - 1] : points[i]
              const p1 = points[i]
              const p2 = points[i + 1]
              const p3 = i < points.length - 2 ? points[i + 2] : p2

              const cp1x = p1.x + (p2.x - p0.x) / 6
              const cp1y = p1.y + (p2.y - p0.y) / 6
              const cp2x = p2.x - (p3.x - p1.x) / 6
              const cp2y = p2.y - (p3.y - p1.y) / 6

              ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, p2.x, p2.y)
            }

            ctx.lineTo(points[points.length - 1].x, paddingTop + chartHeight)
            ctx.closePath()
            ctx.fill()

            ctx.strokeStyle = '#2563eb'
            ctx.lineWidth = 3
            ctx.lineCap = 'round'
            ctx.lineJoin = 'round'
            ctx.beginPath()
            ctx.moveTo(points[0].x, points[0].y)

            // 绘制平滑折线
            for (let i = 0; i < points.length - 1; i++) {
              const p0 = i > 0 ? points[i - 1] : points[i]
              const p1 = points[i]
              const p2 = points[i + 1]
              const p3 = i < points.length - 2 ? points[i + 2] : p2

              const cp1x = p1.x + (p2.x - p0.x) / 6
              const cp1y = p1.y + (p2.y - p0.y) / 6
              const cp2x = p2.x - (p3.x - p1.x) / 6
              const cp2y = p2.y - (p3.y - p1.y) / 6

              ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, p2.x, p2.y)
            }
            ctx.stroke()

            points.forEach(p => {
              ctx.fillStyle = '#2563eb'
              ctx.beginPath()
              ctx.arc(p.x, p.y, 6, 0, Math.PI * 2)
              ctx.fill()

              ctx.fillStyle = '#ffffff'
              ctx.beginPath()
              ctx.arc(p.x, p.y, 3, 0, Math.PI * 2)
              ctx.fill()
            })

            ctx.fillStyle = '#64748b'
            ctx.font = '18px sans-serif'
            ctx.textAlign = 'center'
            ctx.textBaseline = 'top'
            points.forEach(p => {
              ctx.fillText(p.label, p.x, paddingTop + chartHeight + 45)
            })
          })
    }
  }
}
</script>

<style lang="scss" scoped>
.data-page {
  min-height: 100vh;
  background: #f8fafc;
  padding-bottom: 120rpx;
}

.header {
  background: #ffffff;
  padding: 40rpx 32rpx 32rpx;
}

.page-title {
  font-size: 44rpx;
  font-weight: 700;
  color: #1e293b;
}

.tab-bar {
  display: flex;
  background: white;
  padding: 0 32rpx 32rpx;
  gap: 16rpx;
}

.tab-item {
  flex: 1;
  padding: 20rpx 0;
  text-align: center;
  background: #f1f5f9;
  border-radius: 20rpx;
  font-size: 28rpx;
  color: #64748b;

  &.active {
    background: #2563eb;
    color: white;
    font-weight: 600;
  }
}

.content {
  padding: 32rpx;
}

.core-stats-section {
  margin-bottom: 32rpx;
}

.core-stats-card {
  background: white;
  border-radius: 32rpx;
  padding: 40rpx 32rpx;
  border: 2rpx solid #e2e8f0;
}

.core-stats-row {
  display: flex;
  gap: 32rpx;
}

.core-stat-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.core-stat-label {
  font-size: 28rpx;
  color: #94a3b8;
}

.core-stat-value {
  display: flex;
  align-items: baseline;
  gap: 4rpx;
}

.core-stat-value .value {
  font-size: 56rpx;
  font-weight: 700;
  color: #1e293b;
}

.core-stat-value .unit {
  font-size: 28rpx;
  color: #64748b;
}

.core-stat-trend {
  display: flex;
  align-items: center;
  gap: 6rpx;
  font-size: 24rpx;

  &.up {
    color: #ea580c;
  }

  &.down {
    color: #16a34a;
  }

  &.stable {
    color: #2563eb;
  }
}

.trend-icon {
  font-size: 28rpx;
}

.trend-text {
  font-size: 24rpx;
}

.chart-section {
  margin-bottom: 32rpx;
}

.chart-card {
  background: white;
  border-radius: 32rpx;
  padding: 32rpx;
  border: 2rpx solid #e2e8f0;
}

.chart-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 24rpx;
}

.chart-container {
  height: 550rpx;
}

.line-canvas {
  width: 100%;
  height: 100%;
}

.grade-section {
  margin-bottom: 32rpx;
}

.grade-card {
  background: white;
  border-radius: 32rpx;
  padding: 32rpx;
  border: 2rpx solid #e2e8f0;
}

.grade-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 24rpx;
}

.grade-list {
  display: flex;
  flex-direction: column;
  gap: 24rpx;
}

.grade-item {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.grade-info {
  display: flex;
  align-items: center;
  gap: 12rpx;
  width: 160rpx;
  flex-shrink: 0;
}

.grade-dot {
  width: 16rpx;
  height: 16rpx;
  border-radius: 50%;
}

.grade-name {
  font-size: 28rpx;
  color: #1e293b;
  font-weight: 500;
}

.grade-bar-container {
  flex: 1;
  height: 24rpx;
  position: relative;
}

.grade-bar-bg {
  position: absolute;
  width: 100%;
  height: 100%;
  background: #f1f5f9;
  border-radius: 12rpx;
}

.grade-bar {
  position: absolute;
  height: 100%;
  border-radius: 12rpx;
  transition: width 0.3s;
}

.grade-duration {
  width: 100rpx;
  text-align: right;
  font-size: 28rpx;
  font-weight: 600;
  color: #1e293b;
}

.hint-section {
  margin-top: 32rpx;
}

.hint-card {
  background: white;
  border-radius: 32rpx;
  padding: 40rpx 32rpx;
  display: flex;
  align-items: center;
  gap: 24rpx;
  border: 2rpx solid #e2e8f0;
}

.hint-icon {
  font-size: 64rpx;
}

.hint-text {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.hint-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #1e293b;
}

.hint-desc {
  font-size: 26rpx;
  color: #64748b;
}
</style>
