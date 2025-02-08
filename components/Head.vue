<template name="Head">
	<view class="head">
		<view class="head_box" v-for="(item,index) in list" :key="item.id">
			<view>
				<!-- <u-gap height="20" bg-color="#f2f2f2"></u-gap> -->
				<!-- <view style="height: 20rpx;background: #f2f2f2;"></view> -->
				<view class="head_left">
					<view style="width:60%;display: flex;">
						<view class="head_img" @click="clickItem(2,index)">
							<image :src="item.avatar ? item.avatar : '/static/logo.png'"></image>
						</view>
						<view style="margin-left: 20rpx;">
							<view class="flex align-center margin-top-xs">
								<view class="head_name">{{item.userName}}</view>
								<!-- <view class="sexicon margin-left-xs" v-if="item.sex==1">
									<u-icon name="man" color="#FFFFFF"></u-icon>
									{{item.age}}
								</view>
								<view class="sexicons margin-left-xs" v-if="item.sex==2">
									<u-icon name="woman" color="#FFFFFF"></u-icon>
									{{item.age}}
								</view> -->
							</view>
							<view class="head_data">{{item.createTime}}</view>
						</view>
					</view>

					<view v-if="item.userId != userId">
						<view class="guanz" v-if="!className&&item.isCare==0" @click="clickItem(9,index)">关注</view>
						<view class="guanz" v-if="!className&&item.isCare==1" @click="clickItem(9,index)">取消关注</view>
					</view>
					
					<view v-if="item.status == 0" style="color: #AC75FE;width: 20%;text-align: right;font-size: 28upx;"
						@click="clickItem(5,index)">
						编辑
					</view>
					<view v-if="item.status == 0" style="width: 25%;text-align: right;font-size: 28upx;color: red;">
						审核中
					</view>
					<view style="color:#AC75FE;width: 20%;text-align: right;font-size: 28upx;"
						v-if="className&&item.status == 2||item.status == 0" @tap="clickItem(4,index)">
						删除
					</view>
				</view>

				<view class="head_conten" @tap="clickItem(0,index)">{{item.trendsContent}}</view>

				<!-- <view v-if="item.videoUrl" class="margin-lr margin-top-sm">
					<video :src="item.videoUrl" style="width: 100%;height: 320rpx;border-radius: 10rpx;"></video>
				</view> -->
				<view class="head_image" @tap="clickItem(0,index)">
					<image v-for="imgs in item.trendsImage" :src="imgs" :class="item.trendsImage.length>1?'img2':'img1'"
						mode="aspectFill"></image>
				</view>
			</view>
			<!-- 携带商品 -->
			<view class="shopbox" v-if="item.titleImg" @click="clickItem(8,index)">
				<image :src="item.titleImg" style="width: 100rpx;height: 100rpx;"></image>
				<view style="width: 84%;" >
					<view class="tit">{{item.brandName}} {{item.modelsName}} {{item.carName}}</view>
					<view class="parice">{{item.money}}
						<text>万</text>
					</view>
				</view>
			</view>
			
			<!-- 分享 评论 点赞  -->
			<view class="user_mk">
				<view class="mk1">
					<view class="mk_num text-sm" style="color: #999999;font-size: 26rpx;">{{item.browseCount}}浏览</view>
				</view>

				<view class="mk2" @tap="clickItem(3,index)">
					<view class="mk_image">
						<image src="../static/pw/zan.png" v-if="item.isLike != 1"></image>
						<image src="../static/pw/zan1.png" v-else></image>
					</view>
					<view class="mk_num">点赞</view>
				</view>

				<view class="mk3" @tap="clickItem(0,index)">
					<view class="mk_image">
						<image src="../static/pw/msg.png" v-if="item.commentsCount < 1"></image>
						<image src="../static/pw/msgon.png" v-if="item.commentsCount > 0"></image>
					</view>
					<view class="mk_num">评论</view>
				</view>

			</view>

			<view class="padding-left-sm padding-bottom-sm" style="color: red;"
				v-if="item.status==2&&item.auditContent">
				拒绝理由：{{item.auditContent}}
			</view>
			<!-- <view class="margin-lr" style="border-bottom: 1rpx solid #e5e5e5;"></view> -->
			<!-- 展开更多 -->
			<!-- <view class="gengduo" v-for="item in item.gengduo" :key="item.id">
				{{item.name}}
				<image :src="item.icon"></image>
			</view> -->
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			//背景颜色
			list: {
				type: Array,
				default: []
			},
			className: {
				type: Boolean,
				default: false
			},
			userId: {
				type: [Number, String],
				default: "",
			},
		},
		methods: {
			clickItem(ind, item) {
				this.$emit('click', {
					index: ind,
					id: item
				});
			}
		}
	}
</script>

<style lang="less">
	page {
		background: #F5F5F5
	}

	.head {
		width: 100%;
		/* margin-bottom: 50rpx; */
		color: #FFFFFF;
		/* background: #F5F5F5 */
	}

	.head_box {
		/* width: 95%; */
		background: #FFFFFF;
		padding-top: 5rpx;
		padding-bottom: 10rpx;
		/* margin: 20rpx; */
		border-radius: 20rpx;
		margin-bottom: 20upx;
		margin: 20rpx 30rpx;
	}

	.head_left {
		width: 90%;
		margin: 0 auto;
		margin-top: 30rpx;
		display: flex;
		justify-content: space-between;
	}

	.head_img image {
		width: 90rpx;
		height: 90rpx;
		border-radius: 100rpx;
	}

	.head_name {
		color: #000000;

		font-size: 28rpx;
		font-weight: bold;
	}

	.head_data {
		margin-top: 5rpx;
		color: #999999;
		font-size: 24rpx;
		width: 280rpx;
	}

	.head_conten {
		width: 90%;
		margin: 0 auto;
		letter-spacing: 2rpx;
		font-size: 28rpx;
		margin-top: 20rpx;
		color: #000000;
	}

	.head_image {
		width: 95%;
		margin: 0 auto;
		margin-top: 20rpx;
	}

	.img1 {
		width: 420rpx;
		height: 320rpx;
		border-radius: 12rpx;
	}

	.img2 {
		width: 195rpx;
		height: 195rpx;
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
		margin-top: 20rpx;
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
		width: 40rpx;
		height: 40rpx;
	}

	.mk_image image {
		width: 40rpx;
		height: 40rpx;
	}

	.mk_num {
		color: #000;
		margin-left: 10rpx;
		/* margin-top: -8rpx; */
	}

	/* 用户评论 */
	.user_pl {
		width: 100%;
		margin-top: 20rpx;
	}

	.pl_name {
		/* // font-size: 26rpx; */
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

	.pl {
		width: 90%;
		margin: 0 auto 30rpx;
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

	.gengduo image {
		width: 16rpx;
		height: 10rpx;
	}

	.sexicon {
		background: #38CAFF;
		border-radius: 10rpx;
		font-size: 24rpx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		padding: 4rpx 10rpx;
		margin-right: 10rpx;
	}

	.sexicons {
		background: #1F7FFF;
		border-radius: 10rpx;
		font-size: 24rpx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		padding: 4rpx 10rpx;
		margin-right: 10rpx;
	}

	.guanz {
		/* width: 110rpx; */
		/* height: 52rpx; */
		border: 2px solid #1F7FFF;
		border-radius: 35rpx;
		font-size: 24rpx;
		font-family: PingFang SC;
		font-weight: bold;
		color: #1F7FFF;
		padding: 10rpx 25rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.shopbox {
		background: #F0F4FA;
		border-radius: 12rpx;
		padding: 10rpx;
		display: flex;
		align-items: center;
		margin: 21rpx 20rpx 0;

		image {
			margin-right: 10rpx;
		}

		.tit {
			font-size: 28rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
			display: -webkit-box;    
			-webkit-box-orient: vertical;    
			-webkit-line-clamp: 2;    
			overflow: hidden;

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