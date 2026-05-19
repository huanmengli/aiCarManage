<style scoped>
.container {
  padding: 2vw;
  background-color: #f5f7fa;
  min-height: 100vh;
}
.notice-card {
  background-color: #fff;
  border-radius: 2vw;
  padding: 3vw;
  margin-bottom: 2vw;
  box-shadow: 0 0 1vw rgba(0,0,0,0.05);
}
.notice-title {
  font-size: 4vw;
  font-weight: bold;
  color: #333;
  margin-bottom: 2vw;
}
.notice-desc {
  font-size: 4vw;
  color: #666;
  line-height: 1.5;
  margin-bottom: 2vw;
}
.notice-meta {
  display: flex;
  justify-content: space-between;
  font-size: 3vw;
  color: #999;
}
.notice-tag {
  color: #1677ff;
}
</style>

<template>
  <view class="container">
    <view class="notice-card" v-for="(item, index) in noticeList" :key="index" @click="navigateTo(`/pages/notice/detail?id=${index}`)">
      <view class="notice-title">{{ item.title }}</view>
      <view class="notice-desc">{{ item.desc }}</view>
      <view class="notice-meta">
        <text>{{ item.time }}</text>
        <text class="notice-tag">{{ item.type }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      noticeList: [
        {
          title: '2026年春季运动会通知',
          desc: '定于5月20日举办春季运动会，请各班提前做好准备。',
          time: '2026-05-15 10:20',
          type: '全校通知'
        },
        {
          title: '高一家长会安排',
          desc: '高一家长会将于本周五下午15:00在各班教室召开。',
          time: '2026-05-14 09:10',
          type: '年级通知'
        },
        {
          title: '高二（3）班补课提醒',
          desc: '本周六上午9:00-11:00进行数学补课，请勿缺席。',
          time: '2026-05-13 16:30',
          type: '班级通知'
        }
      ]
    };
  }
};
</script>