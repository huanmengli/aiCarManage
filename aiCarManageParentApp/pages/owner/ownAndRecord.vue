<!-- 查看接送记录页面 -->
<template>
	<view class="page">
		<view class="summary-card">
			<view>
				<text class="summary-title">接送记录</text>
				<text class="summary-desc">当前学生：{{ studentInfo.name }} · {{ studentInfo.className }}</text>
			</view>
			<view class="summary-count">
				<text class="count-num">{{ filteredRecords.length }}</text>
				<text class="count-label">条记录</text>
			</view>
		</view>

		<view class="filter-bar">
			<view
				class="filter-item"
				:class="{ active: currentType === item.value }"
				v-for="item in recordTypes"
				:key="item.value"
				@click="changeType(item.value)"
			>
				<text>{{ item.label }}</text>
			</view>
		</view>

		<view class="record-list" v-if="filteredRecords.length">
			<view class="record-card" v-for="item in filteredRecords" :key="item.id">
				<view class="record-top">
					<view>
						<text class="record-title">{{ item.type }}</text>
						<text class="record-time">{{ item.date }} {{ item.time }}</text>
					</view>
					<text class="record-status">{{ item.status }}</text>
				</view>
				<view class="record-info">
					<view class="info-row">
						<text class="info-label">学生</text>
						<text class="info-value">{{ item.studentName }}</text>
					</view>
					<view class="info-row">
						<text class="info-label">接送人</text>
						<text class="info-value">{{ item.pickupName }}（{{ item.relation }}）</text>
					</view>
					<view class="info-row">
						<text class="info-label">车辆</text>
						<text class="info-value">{{ item.carNo || '未登记车辆' }}</text>
					</view>
					<view class="info-row">
						<text class="info-label">地点</text>
						<text class="info-value">{{ item.location }}</text>
					</view>
				</view>
			</view>
		</view>

		<view class="empty" v-else>
			<text class="empty-title">暂无记录</text>
			<text class="empty-desc">切换其他类型或稍后再查看</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				studentInfo: {
					id: 1001,
					name: '李明',
					className: '三年级二班'
				},
				currentType: 'all',
				recordTypes: [
					{ label: '全部', value: 'all' },
					{ label: '到校', value: '到校签到' },
					{ label: '离校', value: '离校接送' }
				],
				records: [
					{
						id: 1,
						type: '到校签到',
						date: '2026-05-16',
						time: '07:42',
						studentName: '李明',
						pickupName: '管女士',
						relation: '母亲',
						carNo: '鲁A·6K219',
						location: '学校东门 1 号车道',
						status: '已完成'
					},
					{
						id: 2,
						type: '离校接送',
						date: '2026-05-15',
						time: '17:18',
						studentName: '李明',
						pickupName: '李先生',
						relation: '父亲',
						carNo: '鲁A·8M536',
						location: '学校东门 2 号车道',
						status: '已完成'
					},
					{
						id: 3,
						type: '到校签到',
						date: '2026-05-15',
						time: '07:39',
						studentName: '李明',
						pickupName: '管女士',
						relation: '母亲',
						carNo: '鲁A·6K219',
						location: '学校东门 1 号车道',
						status: '已完成'
					},
					{
						id: 4,
						type: '离校接送',
						date: '2026-05-14',
						time: '17:05',
						studentName: '李明',
						pickupName: '王阿姨',
						relation: '外婆',
						carNo: '',
						location: '学校南门临停点',
						status: '已完成'
					}
				]
			}
		},
		computed: {
			filteredRecords() {
				if (this.currentType === 'all') {
					return this.records
				}
				return this.records.filter(item => item.type === this.currentType)
			}
		},
		methods: {
			changeType(type) {
				this.currentType = type
			}
		}
	}
</script>

<style>
	page {
		background: #f5f7fb;
	}

	.page {
		min-height: 100vh;
		padding: 28rpx;
		box-sizing: border-box;
		color: #1f2937;
	}

	.summary-card {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 32rpx 28rpx;
		background: #ffffff;
		border-radius: 18rpx;
		box-shadow: 0 16rpx 42rpx rgba(31, 41, 55, 0.08);
	}

	.summary-title {
		display: block;
		font-size: 40rpx;
		font-weight: 800;
		color: #111827;
	}

	.summary-desc {
		display: block;
		margin-top: 12rpx;
		font-size: 25rpx;
		color: #6b7280;
	}

	.summary-count {
		width: 128rpx;
		height: 128rpx;
		border-radius: 18rpx;
		background: #eef2ff;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		color: #3730a3;
		flex-shrink: 0;
	}

	.count-num {
		font-size: 38rpx;
		font-weight: 800;
	}

	.count-label {
		margin-top: 4rpx;
		font-size: 22rpx;
	}

	.filter-bar {
		display: flex;
		gap: 16rpx;
		margin: 24rpx 0;
	}

	.filter-item {
		flex: 1;
		height: 72rpx;
		line-height: 72rpx;
		text-align: center;
		border-radius: 14rpx;
		background: #ffffff;
		color: #4b5563;
		font-size: 27rpx;
	}

	.filter-item.active {
		background: #2563eb;
		color: #ffffff;
		font-weight: 700;
	}

	.record-card {
		margin-bottom: 22rpx;
		padding: 28rpx;
		background: #ffffff;
		border-radius: 18rpx;
		box-shadow: 0 12rpx 34rpx rgba(31, 41, 55, 0.06);
	}

	.record-top {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		padding-bottom: 22rpx;
		border-bottom: 1rpx solid #eef2f7;
	}

	.record-title {
		display: block;
		font-size: 32rpx;
		font-weight: 700;
		color: #111827;
	}

	.record-time {
		display: block;
		margin-top: 8rpx;
		font-size: 24rpx;
		color: #6b7280;
	}

	.record-status {
		padding: 8rpx 16rpx;
		border-radius: 999rpx;
		background: #ecfdf5;
		color: #047857;
		font-size: 23rpx;
	}

	.record-info {
		padding-top: 10rpx;
	}

	.info-row {
		display: flex;
		justify-content: space-between;
		align-items: center;
		min-height: 58rpx;
	}

	.info-label {
		font-size: 25rpx;
		color: #6b7280;
	}

	.info-value {
		max-width: 460rpx;
		text-align: right;
		font-size: 26rpx;
		color: #111827;
	}

	.empty {
		margin-top: 120rpx;
		text-align: center;
		color: #6b7280;
	}

	.empty-title {
		display: block;
		font-size: 32rpx;
		font-weight: 700;
		color: #374151;
	}

	.empty-desc {
		display: block;
		margin-top: 14rpx;
		font-size: 25rpx;
	}
</style>
