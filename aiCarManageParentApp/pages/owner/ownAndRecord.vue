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
		padding: 3.733vw;
		box-sizing: border-box;
		color: #1f2937;
	}

	.summary-card {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 4.267vw 3.733vw;
		background: #ffffff;
		border-radius: 2.4vw;
		box-shadow: 0 2.133vw 5.6vw rgba(31, 41, 55, 0.08);
	}

	.summary-title {
		display: block;
		font-size: 5.333vw;
		font-weight: 800;
		color: #111827;
	}

	.summary-desc {
		display: block;
		margin-top: 1.6vw;
		font-size: 3.333vw;
		color: #6b7280;
	}

	.summary-count {
		width: 17.067vw;
		height: 17.067vw;
		border-radius: 2.4vw;
		background: #eef2ff;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		color: #3730a3;
		flex-shrink: 0;
	}

	.count-num {
		font-size: 5.067vw;
		font-weight: 800;
	}

	.count-label {
		margin-top: 0.533vw;
		font-size: 2.933vw;
	}

	.filter-bar {
		display: flex;
		gap: 2.133vw;
		margin: 3.2vw 0;
	}

	.filter-item {
		flex: 1;
		height: 9.6vw;
		line-height: 9.6vw;
		text-align: center;
		border-radius: 1.867vw;
		background: #ffffff;
		color: #4b5563;
		font-size: 3.6vw;
	}

	.filter-item.active {
		background: #2563eb;
		color: #ffffff;
		font-weight: 700;
	}

	.record-card {
		margin-bottom: 2.933vw;
		padding: 3.733vw;
		background: #ffffff;
		border-radius: 2.4vw;
		box-shadow: 0 1.6vw 4.533vw rgba(31, 41, 55, 0.06);
	}

	.record-top {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		padding-bottom: 2.933vw;
		border-bottom: 0.133vw solid #eef2f7;
	}

	.record-title {
		display: block;
		font-size: 4.267vw;
		font-weight: 700;
		color: #111827;
	}

	.record-time {
		display: block;
		margin-top: 1.067vw;
		font-size: 3.2vw;
		color: #6b7280;
	}

	.record-status {
		padding: 1.067vw 2.133vw;
		border-radius: 133.2vw;
		background: #ecfdf5;
		color: #047857;
		font-size: 3.067vw;
	}

	.record-info {
		padding-top: 1.333vw;
	}

	.info-row {
		display: flex;
		justify-content: space-between;
		align-items: center;
		min-height: 7.733vw;
	}

	.info-label {
		font-size: 3.333vw;
		color: #6b7280;
	}

	.info-value {
		max-width: 61.333vw;
		text-align: right;
		font-size: 3.467vw;
		color: #111827;
	}

	.empty {
		margin-top: 16vw;
		text-align: center;
		color: #6b7280;
	}

	.empty-title {
		display: block;
		font-size: 4.267vw;
		font-weight: 700;
		color: #374151;
	}

	.empty-desc {
		display: block;
		margin-top: 1.867vw;
		font-size: 3.333vw;
	}
</style>
