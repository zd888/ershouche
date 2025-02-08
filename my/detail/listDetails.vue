<template name="Head">
	<view class="head">
		<view class="head_box">
			<view>
				<view class="head_left" >
					<view style="width: 80%;display: flex;">
						<view class="head_img" @tap.stop="goGuanZhuDetail(item.userId)">
							<image :src="item.avatar ? item.avatar : '/static/logo.png'"></image>
						</view>
						<view style="margin-left: 20rpx;">
							<view class="head_name">{{item.userName}}</view>
							<view class="head_data">{{item.createTime}}</view>
						</view>
					</view>
					<!-- <view v-if="item.userId != userId" style="width: 20%;text-align: right;margin-left: 20rpx;"
						@tap="goTousu(item.userId)">
						<image src="../../../static/images/index/tousu.png"
							style="width: 40rpx;height: 40rpx;margin-top: 25upx;"></image>
					</view> -->
				</view>

				<view class="head_conten">{{item.trendsContent}}</view>
				<view v-if="item.videoUrl" class="margin-lr margin-top-sm">
					<video :src="item.videoUrl" style="width: 100%;height: 320rpx;border-radius: 10rpx;"></video>
				</view>

				<view class="head_image">
					<image @tap="selectImage(item.trendsImage,index)" v-for="(imgs,index) in item.trendsImage"
						:src="imgs" :class="item.trendsImage.length>1?'img2':'img1'" mode="aspectFill"></image>
				</view>
			</view>

			<!-- 携带商品 -->
			<view class="shopbox" v-if="item.titleImg" @click="godetail">
				<image :src="item.titleImg" style="width:100rpx;height: 100rpx;" ></image>
				<view>
					<view class="tit">{{item.brandName}} {{item.modelsName}} {{item.carName}}</view>
					<view class="parice">{{item.money}}
						<text>万</text>
					</view>
				</view>
			</view>

			<!-- 分享 评论 点赞  -->
			<view class="user_mk">
				<view class="mk1">
					<!-- <view class="mk_image">
						<image src="../../../static/pw/zan.png" v-if="item.goodTalkByUserCount != 1"></image>
						<image src="../../../static/pw/zan1.png" v-if="item.goodTalkByUserCount == 1"></image>
					</view>
					<view class="mk_num">{{item.goodTalkCount ? item.goodTalkCount : 0}}</view> -->

					<view class="mk_num text-sm" style="color: #999999;font-size: 26rpx;">{{item.browseCount}}浏览</view>
				</view>
				<view class="mk2" @tap="saveLove(item.trendsId)">
					<view class="mk_image">
						<image src="../../static/pw/zan.png" v-if="item.isLike != 1"></image>
						<image src="../../static/pw/zan1.png" v-else></image>
					</view>
					<view class="mk_num">{{item.likeCount ? item.likeCount : 0}}</view>
					<!-- <view class="mk_num " style="color: #999999;font-size: 26rpx;">{{item.likeCount}}点赞</view> -->
				</view>
				<view class="mk3">
					<view class="mk_image">
						<image src="../../static/pw/msg.png" v-if="item.commentsCount < 1"></image>
						<image src="../../static/pw/msgon.png" v-if="item.commentsCount > 0"></image>
					</view>
					<view class="mk_num">{{item.commentsCount ? item.commentsCount : 0}}</view>
					<!-- <view class="mk_num " style="color: #999999;font-size: 26rpx;">{{item.commentsCount}}评论</view> -->
				</view>
			</view>
			<!-- <view style="display: flex;margin-left: 30rpx;align-items: center;" v-if="reward.length > 0">
				<view style="display: flex;margin-left: 15rpx;" v-for="(item,index) in reward">
					<image :src="item.imageUrl ? item.imageUrl : '/static/logo.png'"
						style="width: 40rpx;height: 40rpx;border-radius: 50rpx;"></image>
				</view>
				<view style="margin-left: 10rpx;color: #999999;">{{item.rewardCount ? item.rewardCount : 0}}人已打赏</view>
			</view> -->
			<!-- 审核内容 -->
			<view class="padding-left-sm padding-bottom-sm" style="color: red;"
				v-if="item.status==2&&item.auditContent">
				拒绝理由：{{item.auditContent}}
			</view>
			<!-- 用户评论 -->
			<view class="user_pl" >
				<view class="pl" v-for="ite in item.commentList">
					<image :src="ite.avatar?ite.avatar:'../../static/logo.png'"
						style="width: 70rpx;height: 70rpx;border-radius: 50rpx;"></image>
					<view class="margin-left-sm">
						<view class="pl_name">
							{{ite.userName}}
						</view>
						<view class="pl_names">{{ite.commentContent}}</view>
					</view>
				</view>
			</view>
			<!-- 展开更多 -->
			<!-- <view class="gengduo" v-for="item in item.gengduo" :key="item.id">
				{{item.name}}
				<image :src="item.icon"></image>
			</view> -->
			<!-- 加载更多提示 -->
			<!-- 	<view class="s-col is-col-24" style="margin-top: 30rpx;" v-if="item">
				<load-more :status="loadingType" :contentText="contentText"></load-more>
			</view> -->
		</view>

		<!-- <tui-modal :show="modal" @cancel="hide" title="打赏用户" :moneyMessage="moneyMessage" place="请输入打赏金额"
			:button="button" @click="handleClick" :moneyType="moneyType" :content="moneycontent">
		</tui-modal>-->

		<!-- 底部聊天输入框 -->
		<view class="input-box" v-if="item.status==1">
			<view style="display: flex;margin-top: 15rpx;width: 100%;">
				<input confirm-type="send" @confirm='setChatSave' type="text" v-model="content"
					style="width: 75%;height: 70rpx;background: #F5F5F5;color: black;margin: 4rpx 10rpx 0;border-radius: 10rpx;padding-left: 10rpx;" />
				<view class="save" @tap='setChatSave'>发送</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentPageindex: 0,
				categoryHeight: '300rpx',
				// modal: false,
				// moneyType: false,
				// moneycontent: '',
				// moneyMessage: '请输入打赏金额',
				// button: [{
				// 		text: '确定',
				// 		type: 'red'
				// 	},
				// 	{
				// 		text: '取消',
				// 		type: 'black'
				// 	}
				// ],
				content: '',
				userId: '',
				item: {},
				list: [],
				reward: [],
				trendsId: 0,
				page: 0,
				size: 20,
				loadingType: 0,
				scrollTop: false,
				contentText: {
					contentdown: '上拉显示更多',
					contentrefresh: '正在加载...',
					contentnomore: '没有更多评论了'
				},


			}
		},
		onLoad(d) {
			this.userId = this.$queue.getData('userId');
			this.trendsId = d.trendsId;
			console.log(this.trendsId)

			this.getListDetail();

		},
		onShow() {

		},
		onShareAppMessage(res) {
			return {
				path: '/package/pages/details/listDetails?talkId=' + this.talkId, //这是为了传参   onload(data){let id=data.id;} 
				title: this.item.content,
				imageUrl: this.item.img[0]
			}
		},
		methods: {
			godetail(){
				uni.navigateTo({
					url: '/pages/index/classify/detail?carId=' + this.item.carId
				})
			},
			goTousu(userId) {
				uni.navigateTo({
					url: '/my/order/complain?byUserId=' + userId + '&platform=3&platformId=' + this.talkId
				});
			},
			goNavs(e) {
				uni.navigateTo({
					url: e
				})
			},



			selectImage(image, index) {
				let _this = this;
				let imgsArray = image;
				// imgsArray[0] = image;
				uni.previewImage({
					current: index,
					urls: imgsArray
				});
			},
			saveShare() {
				this.$Request.getT('/app/gambit/addShare?talkId=' + this.talkId).then(res => {
					if (res.status == 0) {
						this.getListDetail();
					}
				});
			},
			setChatSave() {

				if (this.content === '') {
					this.$queue.showToast('评论不能为空！');
					return;
				}
				// if (this.$queue.getChatSearchKeys(this.content)) {
				// 	uni.showToast({
				// 		title: "输入内容带有非法关键字请重新输入",
				// 		mask: false,
				// 		duration: 1500,
				// 		icon: "none"
				// 	});
				// 	this.$Request.postT('/app/risk/insertRisk?riskType=3&content=' + this.content).then(res => {

				// 	})
				// 	return;
				// }
				let userId = this.$queue.getData('userId');
				let data = {
					trendsId: this.trendsId,
					commentContent: this.content
				}
				this.$Request.postT('/app/trendsComment/saveTrendsComment', data).then(res => {
					if (res.code == 0) {
						this.content = ''
						this.getListDetail();
					} else {
						this.$queue.showToast(res.msg);
					}
				});
			},
			saveLove(trendsId) {

				let userId = this.$queue.getData('userId');
				let data = {
					trendsId: trendsId,
					// userId: userId
				}
				this.$Request.postT('/app/trendsLike/saveTrendsLike', data).then(res => {
					if (res.code == 0) {
						this.getListDetail();
					} else {
						this.$queue.showToast(res.msg);
					}
				});
			},
			getDaShang() {
				let userId = this.$queue.getData('userId');
				this.$Request.getT('/app/gambit/selectTalkRewardPage?page=0&size=5&talkId=' + this
					.talkId).then(res => {
					if (res.status === 0) {
						this.reward = res.data.content;
					}
				});
			},

			getListDetail() {
				uni.showLoading({
					title: '加载中...'
				});
				let userId = this.$queue.getData('userId');
				this.$Request.getT('/app/trends/getTrendsInfo?trendsId=' + this.trendsId + '&byUserId=' + userId).then(
					res => {
						if (res.code == 0) {
							uni.hideLoading();
							if (res.data.trendsImage) {
								res.data.trendsImage = res.data.trendsImage.split(',');
							}
							if (res.data.titleImg) {
								let titleImg = res.data.titleImg.split(',');
								res.data.titleImg = titleImg[0]
							}
							this.item = res.data;
						}
					});
			},
			topScrollTap: function() {
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 300
				});
			},

			goGuanZhuDetail(userId) {
				uni.navigateTo({
					url: '/my/gird/guanzhuDetail?userId=' + userId
				});

			}
		},
		onPageScroll: function(e) {
			this.scrollTop = e.scrollTop > 200;
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			this.getPingLun();
		},
		onPullDownRefresh: function() {
			this.page = 0;
			this.getPingLun();
		}
	}
</script>

<style lang="scss">
	page {
		background: #F5F5F5;
	}

	.head {
		width: 100%;
		margin-bottom: 50rpx;
	}

	.head_box {
		width: 100%;
		background: #FFFFFF;
		padding-top: 20rpx;
		margin-bottom: 200rpx;
	}

	.head_left {
		width: 90%;
		margin: 0 auto;
		display: flex;
	}

	.head_img image {
		width: 90rpx;
		height: 90rpx;
		border-radius: 100rpx;
	}

	.head_name {
		margin-top: 10rpx;
		font-size: 28rpx;
		color: #000000;
		font-weight: bold;
	}

	.head_data {
		margin-top: 15rpx;
		color: #999999;
		font-size: 24rpx;
	}

	.head_conten {
		width: 90%;
		margin: 0 auto;
		letter-spacing: 2rpx;
		font-size: 28rpx;
		margin-top: 20rpx;
	}

	.head_image {
		width: 90%;
		margin: 0 auto;
		margin-top: 20rpx;
	}

	.img1 {
		width: 420rpx;
		height: 320rpx;
		border-radius: 12rpx;
	}

	.img2 {
		width: 202rpx;
		height: 202rpx;
		margin-top: 15rpx;
		margin-right: 5rpx;
		margin-left: 15rpx;
		border-radius: 12rpx;
	}

	/* 分享 评论 点赞 */
	.user_mk {
		width: 90%;
		margin: 0 auto;
		display: flex;
		margin-top: 0rpx;
	}

	.mk1 {
		flex: 1;
		height: 80rpx;
		display: flex;
		justify-content: left;
		align-items: center;
	}

	.mk2 {
		flex: 1;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.mk3 {
		flex: 1;
		display: flex;
		justify-content: flex-end;
		align-items: center;
	}

	.mk_image {
		width: 50rpx;
		height: 50rpx;
	}

	.mk_image image {
		width: 46rpx;
		height: 46rpx;
	}

	.mk_num {
		margin-top: -8rpx;
		color: #000;
		margin-left: 10rpx;
	}

	/* 用户评论 */
	.user_pl {
		width: 100%;
		margin-top: 20rpx;
		padding-bottom: 30rpx;
	}

	.pl_name {
		// font-size: 26rpx;
		font-family: PingFang SC Bold, PingFang SC Bold-Bold;
		font-weight: 700;
		text-align: left;
		color: #333333;
		line-height: 32rpx;
	}

	.pl_names {
		font-size: 24rpx;
		font-family: PingFang SC Bold, PingFang SC Bold-Bold;
		// font-weight: 700;
		text-align: left;
		color: #999999;
		line-height: 32rpx;
	}

	button::after {
		border: none;
	}

	.pl {
		width: 90%;
		margin: 0 auto 40rpx;
		display: flex;
		justify-content: left;
		align-items: center;
		height: 50rpx;
		font-size: 24rpx;
		color: #999999;

	}

	.gengduo {
		height: 80rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 20rpx;
		color: #999999;
		letter-spacing: 2rpx;
	}

	.input-box {
		position: fixed;
		bottom: 0;
		left: 0;
		height: 100rpx;
		width: 100%;
		display: flex;
		box-sizing: content-box;
		z-index: 999;
		background-color: #FFFFFF;
		padding: 5rpx;
	}

	.save {
		width: 130rpx;
		text-align: center;
		border-radius: 10rpx;
		height: 70rpx;
		color: #FFFFFF;
		background: #3390FF;
		margin: 5rpx 10rpx 0;
		line-height: 70rpx;
	}

	.gengduo image {
		width: 16rpx;
		height: 10rpx;
	}

	.shopbox {
		background: #F0F4FA;
		border-radius: 12rpx;
		padding: 10rpx;
		display: flex;
		align-items: center;
		margin: 21rpx 30rpx 0;

		image {
			margin-right: 10rpx;
		}

		.tit {
			font-size: 28rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
		}

		.parice {
			font-size: 32rpx;
			font-family: DIN;
			font-weight: bold;
			color: #FF7315;

			text {
				font-size: 24rpx;
			}
		}
	}
</style>