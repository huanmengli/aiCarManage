<!-- 接送人管理子页面 -->
<template>
	<view class="page">
		<view class="top-card">
			<view>
				<text class="page-title">接送人管理</text>
				<text class="page-desc">维护可接送学生的家长与亲属信息</text>
			</view>
			<button class="add-btn" @click="openForm()">新增</button>
		</view>

		<view class="student-card">
			<view class="student-avatar">
				<text>{{ studentInfo.name.slice(0, 1) }}</text>
			</view>
			<view>
				<text class="student-name">{{ studentInfo.name }}</text>
				<text class="student-class">{{ studentInfo.className }}</text>
			</view>
		</view>

		<view class="pickup-card" v-for="item in pickupPeople" :key="item.id">
			<view class="pickup-head">
				<view class="pickup-main">
					<text class="pickup-name">{{ item.name }}</text>
					<text class="relation">{{ item.relation }}</text>
					<text class="default-tag" v-if="item.isDefault">默认</text>
				</view>
				<text class="state" :class="{ disabled: !item.enabled }">{{ item.enabled ? '启用' : '停用' }}</text>
			</view>

			<view class="pickup-info">
				<view class="info-row">
					<text class="info-label">手机号</text>
					<text class="info-value">{{ item.phone }}</text>
				</view>
				<view class="info-row">
					<text class="info-label">车辆</text>
					<text class="info-value">{{ item.carNo || '未登记车辆' }}</text>
				</view>
				<view class="info-row">
					<text class="info-label">身份状态</text>
					<text class="info-value">{{ item.verified ? '已认证' : '待认证' }}</text>
				</view>
			</view>

			<view class="actions">
				<button class="text-btn" @click="setDefault(item.id)" v-if="!item.isDefault">设为默认</button>
				<button class="text-btn" @click="toggleEnabled(item.id)">{{ item.enabled ? '停用' : '启用' }}</button>
				<button class="text-btn" @click="openForm(item)">编辑</button>
				<button class="text-btn danger" @click="removePickup(item.id)">删除</button>
			</view>
		</view>

		<view class="form-mask" v-if="showForm" @click="closeForm">
			<view class="form-panel" @click.stop>
				<view class="form-head">
					<text class="form-title">{{ editingId ? '编辑接送人' : '新增接送人' }}</text>
					<text class="close" @click="closeForm">关闭</text>
				</view>

				<view class="field">
					<text class="field-label">姓名</text>
					<input class="field-input" v-model="form.name" placeholder="请输入姓名" />
				</view>
				<view class="field">
					<text class="field-label">关系</text>
					<input class="field-input" v-model="form.relation" placeholder="如：父亲、母亲、外婆" />
				</view>
				<view class="field">
					<text class="field-label">手机号</text>
					<input class="field-input" v-model="form.phone" type="number" placeholder="请输入手机号" />
				</view>
				<view class="field">
					<text class="field-label">车牌号</text>
					<input class="field-input" v-model="form.carNo" placeholder="可选" />
				</view>
				<view class="switch-row">
					<text class="field-label">设为默认接送人</text>
					<switch :checked="form.isDefault" color="#2563eb" @change="onDefaultChange" />
				</view>

				<button class="save-btn" @click="savePickup">保存</button>
			</view>
		</view>
	</view>
</template>

<script>
	const emptyForm = () => ({
		name: '',
		relation: '',
		phone: '',
		carNo: '',
		isDefault: false
	})

	export default {
		data() {
			return {
				studentInfo: {
					id: 1001,
					name: '李明',
					className: '三年级二班'
				},
				showForm: false,
				editingId: null,
				form: emptyForm(),
				pickupPeople: [
					{ id: 1, name: '管女士', relation: '母亲', phone: '13800000001', carNo: '鲁A·6K219', verified: true, enabled: true, isDefault: true },
					{ id: 2, name: '李先生', relation: '父亲', phone: '13800000002', carNo: '鲁A·8M536', verified: true, enabled: true, isDefault: false },
					{ id: 3, name: '王阿姨', relation: '外婆', phone: '13800000003', carNo: '', verified: false, enabled: true, isDefault: false }
				]
			}
		},
		methods: {
			openForm(item) {
				if (item) {
					this.editingId = item.id
					this.form = {
						name: item.name,
						relation: item.relation,
						phone: item.phone,
						carNo: item.carNo,
						isDefault: item.isDefault
					}
				} else {
					this.editingId = null
					this.form = emptyForm()
				}
				this.showForm = true
			},
			closeForm() {
				this.showForm = false
			},
			onDefaultChange(event) {
				this.form.isDefault = event.detail.value
			},
			savePickup() {
				if (!this.form.name || !this.form.relation || !this.form.phone) {
					uni.showToast({
						title: '请填写姓名、关系和手机号',
						icon: 'none'
					})
					return
				}

				if (this.form.isDefault) {
					this.pickupPeople = this.pickupPeople.map(item => ({
						...item,
						isDefault: false
					}))
				}

				if (this.editingId) {
					this.pickupPeople = this.pickupPeople.map(item => {
						if (item.id !== this.editingId) {
							return item
						}
						return {
							...item,
							name: this.form.name,
							relation: this.form.relation,
							phone: this.form.phone,
							carNo: this.form.carNo,
							isDefault: this.form.isDefault
						}
					})
				} else {
					this.pickupPeople.unshift({
						id: Date.now(),
						name: this.form.name,
						relation: this.form.relation,
						phone: this.form.phone,
						carNo: this.form.carNo,
						verified: false,
						enabled: true,
						isDefault: this.form.isDefault
					})
				}

				this.showForm = false
				uni.showToast({
					title: '保存成功',
					icon: 'success'
				})
			},
			setDefault(id) {
				this.pickupPeople = this.pickupPeople.map(item => ({
					...item,
					isDefault: item.id === id
				}))
			},
			toggleEnabled(id) {
				this.pickupPeople = this.pickupPeople.map(item => {
					if (item.id !== id) {
						return item
					}
					return {
						...item,
						enabled: !item.enabled
					}
				})
			},
			removePickup(id) {
				const target = this.pickupPeople.find(item => item.id === id)
				if (target && target.isDefault) {
					uni.showToast({
						title: '默认接送人不能删除',
						icon: 'none'
					})
					return
				}
				this.pickupPeople = this.pickupPeople.filter(item => item.id !== id)
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

	.top-card,
	.student-card,
	.pickup-card {
		background: #ffffff;
		border-radius: 18rpx;
		box-shadow: 0 14rpx 38rpx rgba(31, 41, 55, 0.07);
	}

	.top-card {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 30rpx 28rpx;
	}

	.page-title {
		display: block;
		font-size: 40rpx;
		font-weight: 800;
		color: #111827;
	}

	.page-desc {
		display: block;
		margin-top: 10rpx;
		font-size: 25rpx;
		color: #6b7280;
	}

	.add-btn {
		width: 132rpx;
		height: 68rpx;
		line-height: 68rpx;
		margin: 0;
		border-radius: 12rpx;
		background: #2563eb;
		color: #ffffff;
		font-size: 27rpx;
	}

	.student-card {
		display: flex;
		align-items: center;
		margin-top: 22rpx;
		padding: 24rpx 28rpx;
	}

	.student-avatar {
		width: 78rpx;
		height: 78rpx;
		border-radius: 50%;
		background: #0f766e;
		color: #ffffff;
		font-size: 34rpx;
		font-weight: 700;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-right: 18rpx;
	}

	.student-name {
		display: block;
		font-size: 31rpx;
		font-weight: 700;
	}

	.student-class {
		display: block;
		margin-top: 6rpx;
		font-size: 24rpx;
		color: #6b7280;
	}

	.pickup-card {
		margin-top: 22rpx;
		padding: 28rpx;
	}

	.pickup-head,
	.pickup-main,
	.actions,
	.info-row,
	.form-head,
	.switch-row {
		display: flex;
		align-items: center;
	}

	.pickup-head,
	.info-row,
	.form-head,
	.switch-row {
		justify-content: space-between;
	}

	.pickup-name {
		font-size: 33rpx;
		font-weight: 700;
		color: #111827;
	}

	.relation,
	.default-tag {
		margin-left: 12rpx;
		padding: 6rpx 12rpx;
		border-radius: 999rpx;
		font-size: 22rpx;
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
		font-size: 24rpx;
		color: #047857;
	}

	.state.disabled {
		color: #9ca3af;
	}

	.pickup-info {
		margin-top: 20rpx;
		padding: 16rpx 0;
		border-top: 1rpx solid #eef2f7;
		border-bottom: 1rpx solid #eef2f7;
	}

	.info-row {
		min-height: 56rpx;
	}

	.info-label {
		color: #6b7280;
		font-size: 25rpx;
	}

	.info-value {
		max-width: 420rpx;
		text-align: right;
		font-size: 26rpx;
		color: #111827;
	}

	.actions {
		justify-content: flex-end;
		gap: 14rpx;
		margin-top: 22rpx;
		flex-wrap: wrap;
	}

	.text-btn {
		height: 58rpx;
		line-height: 58rpx;
		margin: 0;
		padding: 0 20rpx;
		border-radius: 10rpx;
		background: #f3f4f6;
		color: #374151;
		font-size: 24rpx;
	}

	.text-btn.danger {
		background: #fef2f2;
		color: #b91c1c;
	}

	.form-mask {
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		z-index: 20;
		background: rgba(17, 24, 39, 0.42);
		display: flex;
		align-items: flex-end;
	}

	.form-panel {
		width: 100%;
		padding: 30rpx 28rpx 48rpx;
		box-sizing: border-box;
		border-radius: 28rpx 28rpx 0 0;
		background: #ffffff;
	}

	.form-title {
		font-size: 34rpx;
		font-weight: 800;
	}

	.close {
		font-size: 26rpx;
		color: #2563eb;
	}

	.field {
		margin-top: 22rpx;
	}

	.field-label {
		font-size: 26rpx;
		color: #4b5563;
	}

	.field-input {
		margin-top: 12rpx;
		height: 78rpx;
		padding: 0 20rpx;
		border-radius: 12rpx;
		background: #f3f4f6;
		font-size: 28rpx;
	}

	.switch-row {
		margin-top: 22rpx;
		min-height: 72rpx;
	}

	.save-btn {
		margin-top: 26rpx;
		height: 88rpx;
		line-height: 88rpx;
		border-radius: 14rpx;
		background: #2563eb;
		color: #ffffff;
		font-size: 30rpx;
		font-weight: 700;
	}
</style>
