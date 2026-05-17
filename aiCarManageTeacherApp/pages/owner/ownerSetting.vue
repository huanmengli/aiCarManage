<template>
  <view class="edit-user">
    <!-- 姓名 -->
    <view class="edit-item">
      <text class="label">姓名</text>
      <input class="inp" v-model="form.name" placeholder="请输入姓名" />
    </view>

    <!-- 年龄 -->
    <view class="edit-item">
      <text class="label">年龄</text>
      <input class="inp" v-model="form.age" type="number" placeholder="请输入年龄" />
    </view>

    <!-- 性别 -->
    <view class="edit-item">
      <text class="label">性别</text>
      <picker :range="genderList" @change="changeGender" :value="genderIndex">
        <view class="picker-text">{{ form.gender }}</view>
      </picker>
    </view>

    <!-- 保存按钮 -->
    <view class="save-btn" @click="save">保存修改</view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      genderList: ["男", "女"],
      genderIndex: 0,
      form: {
        name: "张老师",
        age: "28",
        gender: "男"
      }
    };
  },
  methods: {
    // 选择性别
    changeGender(e) {
      this.genderIndex = e.detail.value;
      this.form.gender = this.genderList[e.detail.value];
    },
    // 保存
    save() {
      const { name, age } = this.form;
      if (!name) {
        uni.showToast({ title: "请输入姓名", icon: "none" });
        return;
      }
      if (!age) {
        uni.showToast({ title: "请输入年龄", icon: "none" });
        return;
      }

      uni.showToast({ title: "保存成功", icon: "success" });
      setTimeout(() => uni.navigateBack(), 1500);
    }
  }
};
</script>

<style scoped>
.edit-user {
  background: #f5f7fa;
  min-height: 100vh;
  padding: 30rpx;
}
.edit-item {
  background: #fff;
  border-radius: 16rpx;
  padding: 0 25rpx;
  margin-bottom: 20rpx;
  display: flex;
  align-items: center;
  height: 95rpx;
}
.label {
  font-size: 30rpx;
  color: #333;
  width: 120rpx;
}
.inp {
  flex: 1;
  font-size: 30rpx;
}
.picker-text {
  flex: 1;
  font-size: 30rpx;
  color: #333;
  text-align: right;
}
.save-btn {
  background: linear-gradient(90deg, #2563eb, #4080ff);
  border-radius: 16rpx;
  text-align: center;
  line-height: 90rpx;
  color: #fff;
  font-size: 33rpx;
  margin-top: 50rpx;
  box-shadow: 0 6rpx 20rpx rgba(37, 99, 235, 0.2);
}
</style>