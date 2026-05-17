<!-- 学生接送人页面 -->
<template>
	<view class="page">
		<view class="student-panel">
			<view class="student-left">
				<view class="avatar">
					<text>{{ studentInfo.name.slice(0, 1) }}</text>
				</view>
				<view class="student-text">
					<text class="student-name">{{ studentInfo.name }}</text>
					<text class="student-desc">{{ studentInfo.gradeName }} · {{ studentInfo.className }}</text>
				</view>
			</view>
			<view class="count-box">
				<text class="count-num">{{ enabledCount }}</text>
				<text class="count-label">可接送</text>
			</view>
		</view>

		<view class="notice-card">
			<text class="notice-title">接送人核对</text>
			<text class="notice-desc">教师端用于查看该学生所有已绑定接送人，便于放学接送时核验身份。</text>
		</view>

		<view class="pickup-list" v-if="pickupPeople.length">
			<view class="pickup-card" v-for="item in pickupPeople" :key="item.id">
				<view class="card-head">
					<view class="person-main">
						<text class="person-name">{{ item.name }}</text>
						<text class="relation">{{ item.relation }}</text>
						<text class="default-tag" v-if="item.isDefault">默认</text>
					</view>
					<text class="state" :class="{ disabled: !item.enabled }">{{ item.enabled ? '允许接送' : '已停用' }}</text>
				</view>

				<view class="detail-grid">
					<view class="detail-row">
						<text class="detail-label">手机号</text>
						<text class="detail-value">{{ item.phone }}</text>
					</view>
					<view class="detail-row">
						<text class="detail-label">车牌号</text>
						<text class="detail-value">{{ item.carNo || '未登记车辆' }}</text>
					</view>
					<view class="detail-row">
						<text class="detail-label">认证状态</text>
						<text class="detail-value">{{ item.verified ? '已认证' : '待认证' }}</text>
					</view>
					<view class="detail-row">
						<text class="detail-label">最近接送</text>
						<text class="detail-value">{{ item.lastPickupTime || '暂无记录' }}</text>
					</view>
				</view>

				<view class="action-row">
					<button class="action-btn" @click="callPhone(item.phone)">联系</button>
					<button class="action-btn ghost" @click="showPickupDetail(item)">查看详情</button>
				</view>
			</view>
		</view>

		<view class="empty" v-else>
			<text class="empty-title">暂无接送人</text>
			<text class="empty-desc">该学生还没有绑定家长或接送人</text>
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
					gradeName: '三年级',
					className: '二班'
				},
				pickupPeople: [
					{ id: 1, name: '管女士', relation: '母亲', phone: '13800000001', carNo: '鲁A·6K219', verified: true, enabled: true, isDefault: true, lastPickupTime: '2026-05-16 07:42' },
					{ id: 2, name: '李先生', relation: '父亲', phone: '13800000002', carNo: '鲁A·8M536', verified: true, enabled: true, isDefault: false, lastPickupTime: '2026-05-15 17:18' },
					{ id: 3, name: '王阿姨', relation: '外婆', phone: '13800000003', carNo: '', verified: false, enabled: true, isDefault: false, lastPickupTime: '2026-05-14 17:05' },
					{ id: 4, name: '刘先生', relation: '舅舅', phone: '13800000004', carNo: '鲁A·3P806', verified: true, enabled: false, isDefault: false, lastPickupTime: '' }
				]
			}
		},
		computed: {
			enabledCount() {
				return this.pickupPeople.filter(item => item.enabled).length
			}
		},
		onLoad(options) {
			if (options.studentId) {
				this.studentInfo.id = options.studentId
			}
			if (options.studentName) {
				this.studentInfo.name = decodeURIComponent(options.studentName)
			}
			if (options.className) {
				this.studentInfo.className = decodeURIComponent(options.className)
			}
		},
		methods: {
			callPhone(phone) {
				uni.makePhoneCall({
					phoneNumber: phone,
					fail: () => {
						uni.showToast({
							title: '已取消拨号',
							icon: 'none'
						})
					}
				})
			},
			showPickupDetail(item) {
				uni.showModal({
					title: item.name,
					content: `关系：${item.relation}\n手机号：${item.phone}\n车牌号：${item.carNo || '未登记车辆'}\n状态：${item.enabled ? '允许接送' : '已停用'}`,
					showCancel: false
				})
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

	.student-panel,
	.notice-card,
	.pickup-card {
		background: #ffffff;
		border-radius: 2.4vw;
		box-shadow: 0 1.867vw 5.067vw rgba(31, 41, 55, 0.07);
	}

	.student-panel {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 4vw 3.733vw;
	}

	.student-left {
		display: flex;
		align-items: center;
		min-width: 0;
	}

	.avatar {
		width: 12vw;
		height: 12vw;
		border-radius: 50%;
		background: #2563eb;
		color: #ffffff;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 5.067vw;
		font-weight: 800;
		flex-shrink: 0;
	}

	.student-text {
		margin-left: 2.4vw;
		min-width: 0;
	}

	.student-name {
		display: block;
		font-size: 5.067vw;
		font-weight: 800;
		color: #111827;
	}

	.student-desc {
		display: block;
		margin-top: 1.067vw;
		font-size: 3.333vw;
		color: #6b7280;
	}

	.count-box {
		width: 16vw;
		height: 16vw;
		border-radius: 2.133vw;
		background: #ecfdf5;
		color: #047857;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
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

	.notice-card {
		margin-top: 2.933vw;
		padding: 3.2vw 3.733vw;
		border-left: 1.067vw solid #2563eb;
	}

	.notice-title {
		display: block;
		font-size: 4vw;
		font-weight: 700;
	}

	.notice-desc {
		display: block;
		margin-top: 1.333vw;
		font-size: 3.333vw;
		line-height: 1.6;
		color: #6b7280;
	}

	.pickup-card {
		margin-top: 2.933vw;
		padding: 3.733vw;
	}

	.card-head,
	.person-main,
	.detail-row,
	.action-row {
		display: flex;
		align-items: center;
	}

	.card-head,
	.detail-row {
		justify-content: space-between;
	}

	.person-name {
		font-size: 4.4vw;
		font-weight: 800;
		color: #111827;
	}

	.relation,
	.default-tag {
		margin-left: 1.6vw;
		padding: 0.8vw 1.6vw;
		border-radius: 133.2vw;
		font-size: 2.933vw;
	}

	.relation {
		background: #eef2ff;
		color: #3730a3;
	}

	.default-tag {
		background: #fff7ed;
		color: #c2410c;
	}

	.state {
		font-size: 3.2vw;
		color: #047857;
	}

	.state.disabled {
		color: #9ca3af;
	}

	.detail-grid {
		margin-top: 2.933vw;
		padding: 1.867vw 0;
		border-top: 0.133vw solid #eef2f7;
		border-bottom: 0.133vw solid #eef2f7;
	}

	.detail-row {
		min-height: 7.733vw;
	}

	.detail-label {
		font-size: 3.333vw;
		color: #6b7280;
	}

	.detail-value {
		max-width: 57.333vw;
		text-align: right;
		font-size: 3.467vw;
		color: #111827;
	}

	.action-row {
		justify-content: flex-end;
		gap: 2.133vw;
		margin-top: 2.933vw;
	}

	.action-btn {
		width: 20vw;
		height: 8.267vw;
		line-height: 8.267vw;
		margin: 0;
		border-radius: 1.6vw;
		background: #2563eb;
		color: #ffffff;
		font-size: 3.333vw;
	}

	.action-btn.ghost {
		background: #f3f4f6;
		color: #374151;
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
