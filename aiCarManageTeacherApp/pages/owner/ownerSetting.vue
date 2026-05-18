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
  padding: 4vw;
}
.edit-item {
  background: #fff;
  border-radius: 2.13vw;
  padding: 0 3.33vw;
  margin-bottom: 2.66vw;
  display: flex;
  align-items: center;
  height: 12.66vw;
}
.label {
  font-size: 4vw;
  color: #333;
  width: 16vw;
}
.inp {
  flex: 1;
  font-size: 4vw;
}
.picker-text {
  flex: 1;
  font-size: 4vw;
  color: #333;
  text-align: right;
}
.save-btn {
  background: linear-gradient(90deg, #2563eb, #4080ff);
  border-radius: 2.13vw;
  text-align: center;
  line-height: 12vw;
  color: #fff;
  font-size: 4.4vw;
  margin-top: 6.66vw;
  box-shadow: 0 0.8vw 2.66vw rgba(37, 99, 235, 0.2);
}
</style>