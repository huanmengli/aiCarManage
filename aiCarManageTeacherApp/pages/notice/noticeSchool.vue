<style scoped>
.container {
  padding: 2vw;
  background-color: #f5f7fa;
  min-height: 100vh;
}
.tab-bar {
  display: flex;
  background-color: #fff;
  border-radius: 2vw;
  margin-bottom: 2vw;
}
.tab-item {
  flex: 1;
  text-align: center;
  padding: 3vw 0;
  font-size: 4vw;
}
.tab-item.active {
  color: #1677ff;
  border-bottom: 1vw solid #1677ff;
}
.form-card {
  background-color: #fff;
  border-radius: 2vw;
  padding: 3vw;
}
.form-item {
  margin-bottom: 3vw;
}
.form-label {
  font-size: 4vw;
  color: #333;
  margin-bottom: 2vw;
}
.input-box {
  width: 100%;
  border: 1px solid #eee;
  border-radius: 1vw;
  padding: 3vw;
  font-size: 4vw;
  box-sizing: border-box;
}
.textarea {
  min-height: 27vw;
}
.btn-submit {
  width: 100%;
  background-color: #1677ff;
  color: #fff;
  text-align: center;
  padding: 3vw 0;
  border-radius: 2vw;
  font-size: 4vw;
}
.select-box {
  display: flex;
  flex-wrap: wrap;
  gap: 2vw;
}
.select-tag {
  background-color: #f0f7ff;
  color: #1677ff;
  padding: 1vw 2vw;
  border-radius: 1vw;
  font-size: 3vw;
}
</style>

<template>
  <view class="container">
    <view class="tab-bar">
      <view class="tab-item" :class="{active: activeTab === 'grade'}">按年级通知</view>
      <view class="tab-item" :class="{active: activeTab === 'class'}">按班级通知</view>
    </view>

    <view class="form-card">
      <view class="form-item">
        <view class="form-label">通知标题</view>
        <input class="input-box" type="text" :placeholder="placeholder.title" />
      </view>

      <view class="form-item">
        <view class="form-label">接收对象</view>
        <view class="select-box">
          <view class="select-tag" v-for="item in targetList" :key="item">{{ item }}</view>
        </view>
      </view>

      <view class="form-item">
        <view class="form-label">通知内容</view>
        <textarea class="input-box textarea" :placeholder="placeholder.content"></textarea>
      </view>

      <view class="btn-submit">发布通知</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 'grade',
      placeholder: {
        title: '请输入通知标题',
        content: '请输入通知详细内容'
      },
      gradeList: ['高一', '高二', '高三'],
      classList: ['高一(1)班', '高一(2)班', '高二(1)班', '高二(2)班', '高三(1)班'],
      targetList: ['高一', '高二']
    };
  }
};
</script>