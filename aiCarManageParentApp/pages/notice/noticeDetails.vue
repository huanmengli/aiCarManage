<style scoped>
.container {
  padding: 2vw;
  background-color: #f5f7fa;
  min-height: 100vh;
}
.detail-card {
  background-color: #fff;
  border-radius: 2vw;
  padding: 4vw;
}
.detail-title {
  font-size: 5vw;
  font-weight: bold;
  color: #333;
  margin-bottom: 3vw;
}
.detail-meta {
  font-size: 3vw;
  color: #999;
  margin-bottom: 4vw;
}
.detail-content {
  font-size: 4vw;
  color: #333;
  line-height: 1.8;
}
</style>

<template>
  <view class="container">
    <view class="detail-card">
      <view class="detail-title">{{ detail.title }}</view>
      <view class="detail-meta">发布时间：{{ detail.time }} &nbsp;&nbsp; 类型：{{ detail.type }}</view>
      <view class="detail-content">{{ detail.content }}</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      detail: {
        title: '2026年春季运动会通知',
        time: '2026-05-15 10:20',
        type: '全校通知',
        content: '为增强学生体质，丰富校园文化生活，学校定于2026年5月20日（星期三）上午8:30在操场举办春季运动会。请各班提前组织报名、训练，运动会当天统一穿校服，准时到场参加开幕式。'
      }
    };
  }
	created() {
		const id = this.getUserId();
		this.loadAllDate(id);
	}

	methods:{
		async loadNotice(userId){
						try {
							uni.showLoading({
								"加载中......"
							});
							await Promise.all([
								  this.loadNoticeData(),
							]);
							uni.hideLoading();
						}catch (error){
							console.error('数据加载失败:', error);
							uni.hideLoading();
							uni.showToast({ title: '数据加载失败，请重试', icon: 'none', duration: 1500 });
							this.setDefaultData(); // 降级显示默认数据
						}finally {
				      userId && userId();
				    }
					},
					async loadNoticeData(userId){
						try {
						        uni.showLoading({ title: '加载中...' })
						        const res = await getParentNoticeList(this.parentId)
						        if (res.code === 200 && res.data) {
						          this.noticeList = res.data.map(item => ({
						            ...item,
						            typeText: this.getTypeText(item.type)
						          }))
						        }
						      } catch (error) {
						        console.error('加载通知列表失败:', error)
						      } finally {
						        uni.hideLoading()
						      }
					},
	}
};
</script>