<!-- 首页签到页面 -->
<template>
	<view class="page">
		<view class="header">
			<view>
				<text class="greeting">{{ greeting }}</text>
				<text class="user-name">{{ parentInfo.name }} {{ parentInfo.relation }}</text>
			</view>
			<view class="notice-btn">
				<text class="notice-icon">铃</text>
			</view>
		</view>

		<view class="sign-card">
			<view class="status-pill" :class="{ signed: hasSigned }">
				<text>状态：{{ hasSigned ? '已签到' : '可签到' }}</text>
			</view>
			<text class="sign-tip">{{ hasSigned ? '今日签到已完成' : '点击下方按钮开始接送签到' }}</text>

			<view class="sign-circle" :class="{ signed: hasSigned }" @click="handleSignIn">
				<view class="pin">
					<view class="pin-dot"></view>
				</view>
				<text class="sign-text">{{ hasSigned ? '已签到' : '立即签到' }}</text>
			</view>

			<text class="location-text">当前位置：{{ currentLocation }}</text>
		</view>

		<view class="stats-grid">
			<view class="stat-card">
				<view class="stat-icon orange">
					<text>日</text>
				</view>
				<text class="stat-label">本月签到</text>
				<view>
					<text class="stat-num">{{ monthSignCount }}</text>
					<text class="stat-unit"> 次</text>
				</view>
			</view>
			<view class="stat-card">
				<view class="stat-icon blue">
					<text>时</text>
				</view>
				<text class="stat-label">平均用时</text>
				<view>
					<text class="stat-num">{{ averageTime }}</text>
					<text class="stat-unit"> min</text>
				</view>
			</view>
		</view>

		<view class="record-section">
			<text class="section-title">最近接送记录</text>
			<view class="record-card" v-for="item in visibleRecentRecords" :key="item.id">
				<view class="record-dot"></view>
				<view class="record-main">
					<text class="record-title">{{ item.date }} {{ item.type }}</text>
					<text class="record-time">{{ item.time }} 签到</text>
				</view>
				<text class="record-cost">耗时 {{ item.cost }}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				parentInfo: {
					name: '张晓明',
					relation: '爸爸'
				},
				currentLocation: '第一实验小学西门',
				hasSigned: false,
				monthSignCount: 22,
				averageTime: '15.4',
				recentRecords: [
					{ id: 1, date: '2026-05-13', type: '接送', time: '下午 16:32', cost: '18min' },
					{ id: 2, date: '2026-05-12', type: '接送', time: '下午 16:28', cost: '16min' },
					{ id: 3, date: '2026-05-11', type: '接送', time: '下午 16:35', cost: '20min' },
					{ id: 4, date: '2026-05-10', type: '接送', time: '下午 16:30', cost: '15min' },
					{ id: 5, date: '2026-05-09', type: '接送', time: '下午 16:26', cost: '14min' },
					{ id: 6, date: '2026-05-08', type: '接送', time: '下午 16:33', cost: '17min' }
				]
			}
		},
		computed: {
			greeting() {
				const hour = new Date().getHours()
				if (hour < 12) {
					return '上午好，'
				}
				if (hour < 18) {
					return '下午好，'
				}
				return '晚上好，'
			},
			visibleRecentRecords() {
				return this.recentRecords.slice(0, 5)
			}
		},
		methods: {
			handleSignIn() {
				if (this.hasSigned) {
					uni.showToast({
						title: '今日已签到',
						icon: 'none'
					})
					return
				}

				const now = new Date()
				const pad = value => String(value).padStart(2, '0')
				const date = `${now.getFullYear()}-${pad(now.getMonth() + 1)}-${pad(now.getDate())}`
				const time = `${now.getHours() < 12 ? '上午' : '下午'} ${pad(now.getHours())}:${pad(now.getMinutes())}`

				this.hasSigned = true
				this.monthSignCount += 1
				this.recentRecords.unshift({
					id: Date.now(),
					date,
					type: '接送',
					time,
					cost: '0min'
				})

				uni.showToast({
					title: '签到成功',
					icon: 'success'
				})
			}
		}
	}
</script>

<style>
	page {
		background: #f5f8fc;
	}

	.page {
		min-height: 100vh;
		padding: 12vw 4.267vw 6.4vw;
		box-sizing: border-box;
		color: #0f172a;
	}

	.header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 4.533vw;
	}

	.greeting {
		display: block;
		font-size: 3.2vw;
		color: #375075;
		line-height: 1.2;
	}

	.user-name {
		display: block;
		margin-top: 0.8vw;
		font-size: 4.8vw;
		font-weight: 800;
		color: #0f172a;
		line-height: 1.2;
	}

	.notice-btn {
		width: 8.8vw;
		height: 8.8vw;
		border-radius: 50%;
		background: #edf4ff;
		color: #2f6bea;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.notice-icon {
		font-size: 3.467vw;
		font-weight: 700;
	}

	.sign-card {
		position: relative;
		padding: 4.8vw 3.733vw 6.933vw;
		border-radius: 4.8vw;
		background: #ffffff;
		box-shadow: 0 3.2vw 8vw rgba(38, 79, 150, 0.08);
		text-align: center;
	}

	.status-pill {
		position: absolute;
		right: 3.2vw;
		top: 3.2vw;
		padding: 1.067vw 2.4vw;
		border-radius: 133.2vw;
		background: #e9fbef;
		color: #16a34a;
		font-size: 2.8vw;
		font-weight: 700;
	}

	.status-pill.signed {
		background: #eef2ff;
		color: #3154d4;
	}

	.sign-tip {
		display: block;
		margin-top: 2.667vw;
		font-size: 3.733vw;
		color: #8a97ad;
	}

	.sign-circle {
		width: 34.667vw;
		height: 34.667vw;
		margin: 7.733vw auto 6.133vw;
		border-radius: 50%;
		background: #2f66e8;
		box-shadow: 0 0 0 2.133vw #f1f5ff, 0 3.2vw 6.667vw rgba(47, 102, 232, 0.28);
		color: #ffffff;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.sign-circle.signed {
		background: #15b981;
		box-shadow: 0 0 0 2.133vw #e9fbef, 0 3.2vw 6.667vw rgba(21, 185, 129, 0.24);
	}

	.pin {
		position: relative;
		width: 6.4vw;
		height: 7.733vw;
		margin-bottom: 2.667vw;
		border: 0.933vw solid #ffffff;
		border-radius: 50% 50% 50% 0;
		transform: rotate(-45deg);
		box-sizing: border-box;
	}

	.pin-dot {
		position: absolute;
		left: 50%;
		top: 50%;
		width: 1.867vw;
		height: 1.867vw;
		margin-left: -0.933vw;
		margin-top: -0.933vw;
		border-radius: 50%;
		background: #ffffff;
	}

	.sign-text {
		font-size: 4.267vw;
		font-weight: 800;
	}

	.location-text {
		display: block;
		font-size: 3.333vw;
		font-weight: 700;
		color: #9aa7bc;
	}

	.stats-grid {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 3.467vw;
		margin-top: 6.933vw;
	}

	.stat-card {
		min-height: 20.533vw;
		padding: 3.733vw 3.467vw;
		border-radius: 2.933vw;
		background: #ffffff;
		box-shadow: 0 2.4vw 5.867vw rgba(38, 79, 150, 0.07);
		box-sizing: border-box;
	}

	.stat-icon {
		width: 5.867vw;
		height: 5.867vw;
		border-radius: 1.6vw;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 2.933vw;
		font-weight: 800;
	}

	.stat-icon.orange {
		background: #fff3e7;
		color: #f97316;
	}

	.stat-icon.blue {
		background: #eef2ff;
		color: #5568f6;
	}

	.stat-label {
		display: block;
		margin-top: 2.933vw;
		font-size: 3.2vw;
		color: #7a8799;
	}

	.stat-num {
		font-size: 4.8vw;
		font-weight: 900;
		color: #0f172a;
	}

	.stat-unit {
		font-size: 3.2vw;
		font-weight: 700;
		color: #0f172a;
	}

	.record-section {
		margin-top: 7.2vw;
	}

	.section-title {
		display: block;
		margin-bottom: 3.2vw;
		font-size: 4vw;
		font-weight: 800;
		color: #172033;
	}

	.record-card {
		display: flex;
		align-items: center;
		min-height: 12.8vw;
		margin-bottom: 2.4vw;
		padding: 2.933vw 3.467vw;
		border-radius: 2.4vw;
		background: #ffffff;
		box-shadow: 0 1.6vw 4.533vw rgba(38, 79, 150, 0.06);
		box-sizing: border-box;
	}

	.record-dot {
		width: 1.6vw;
		height: 1.6vw;
		margin-right: 2.667vw;
		border-radius: 50%;
		background: #2f66e8;
		flex-shrink: 0;
	}

	.record-main {
		flex: 1;
		min-width: 0;
	}

	.record-title {
		display: block;
		font-size: 3.467vw;
		font-weight: 800;
		color: #1f2a44;
	}

	.record-time {
		display: block;
		margin-top: 0.8vw;
		font-size: 3.067vw;
		color: #99a5b8;
	}

	.record-cost {
		margin-left: 2.667vw;
		font-size: 3.067vw;
		font-weight: 800;
		color: #344055;
		flex-shrink: 0;
	}
</style>
