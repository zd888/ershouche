<template>
	<view class="container">
		<view class="contop">
			<view style="width: 20%;">
				<view :class="current==1?'select':''" @click="confirm(1)">推荐</view>
			</view>
			<view style="width:55%;">
				<view :class="current==2?'select':''" @click="confirm(2)">关注</view>
			</view>
			<view class="button" @click="fabu">
				<text>发动态</text>
			</view>
		</view>
		<view class="evaluate">
			<view class="evaluateCenter">
				<view class="evaluateItem" v-for="(item, index) in list" :key="index">
					<view class="evaluateTip  flex">
						<view class="img">
							<image :src="item.image" mode="widthFix" @click="goUserInfo"></image>
						</view>
						<view class="right">
							<view class="righttip">{{ item.name}}</view>
							<view class="bottom ">
								<view class="time">{{item.time}}</view>
							</view>
						</view>
						<view class="buttonn" style="margin-top: 10rpx;" v-if="item.stats==1">
							<text>关注</text>
						</view>
						<view class="buttonn" style="margin-top: 10rpx;" v-if="item.stats==2">
							<text>取关</text>
						</view>
					</view>
					<view class="evaluateText" :class="{lineclamp3:item.contentAll}"> {{item.content}} </view>
					<view class="" v-if="item.isMore">
						<view class="rightText" v-if="item.contentAll" @click="changeAllFun(item, index)">全部</view>
						<view class="rightText" v-else @click="changeAllFun(item, index)">收起</view>
					</view>
					<view class="evaluateListImg">
						<view class="evaluateListImgItem ">
							<image :src="item.image" style="width: 356rpx; height: 429rpx;"></image>
						</view>
					</view>
					<view class="bottomsp flex">
						<view class="bottomspimg">
							<image :src="item.carlistimg" mode="widthFix"></image>
						</view>
						<view class="bottomspcont">
							<view class="bottomspcont_name">
								{{item.carlistname}}
							</view>
							<view class="bottomspcont_name2">
								<text>{{item.carlistmoney}}</text>
								<text style="font-size: 24rpx; margin-right: 10rpx;">万</text>
								<text
									style="font-size: 22rpx;font-family: PingFang SC;color: #9D9EAB;">{{item.carlistshoufu}}万首付</text>
							</view>
						</view>

					</view>
					<!--  <view class="bottomImg">
            <view style="margin-left:20rpx ;">
              <text>{{item.num}}浏览</text>
            </view>
            <view class="flex">
              <image src="../../static/pw/zan.png" mode="scaleToFill" style="margin-right: 5rpx;"></image>
              <text style="margin-top: 4rpx;">点赞</text>
            </view>
            <view class="flex" @click="goto()">
              <image src="../../static/news.png" mode="scaleToFill" style="margin-right: 10rpx;"></image>
              <text>评论</text>
            </view>

          </view> -->
				</view>
				<empty v-if="list.length==0" />
			</view>
		</view>
	</view>
</template>

<script>
	import empty from '../../components/empty.vue'
	export default {
		components: {
			empty
		},
		data() {
			return {
				current: 1,
				list: []
			};
		},
		created() {
			this.getlist()
		},
		onLoad() {
			this.getlist()
		},
		methods: {
			confirm(e) {
				this.page = 1
				this.current = e;
				if (this.current == 2) {
					// this.getdemo()
				}
			},
			goto() {
				uni.navigateTo({
					url: '/my/feedback/dongtai'
				})
			},
			goUserInfo() {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/my/userInfo/userInfo'
					})
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			fabu() {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/my/userInfo/fabu'
					})
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			getlist() {
				this.list.forEach(item => {
					if (item.content.length > 60) {
						item.isMore = true
						item.contentAll = true
					} else {
						item.isMore = false
						item.contentAll = false
					}
				})
			},
			changeAllFun(item, index) {
				let list = JSON.parse(JSON.stringify(this.list));
				list.forEach((elem, ind) => {
					if (index === ind) {
						elem.contentAll = !elem.contentAll
					}
				})
				this.list = list
			}
		}

	}
</script>

<style lang="scss" scoped>
	.select {
		font-size: 40rpx;
		font-family: PingFang SC;
		font-weight: 800;
		color: #333333;
	}

	.container {
		width: 100vw;
		font-size: 24rpx;
		display: inline-block;
		color: #1d1d1d;
		position: relative;
		background-image: url('../../static/images/my/bj.png');
		height: 346rpx;
		background-repeat: no-repeat;
		background-size: 100%;

		.contop {
			display: flex;
			padding: 48rpx 40rpx;
			font-size: 34rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #999999;
			align-items: flex-end;

			.button {
				width: 170rpx;
				height: 70rpx;
				background: linear-gradient(51deg, #1F7BFF, #21B2FF);
				border-radius: 35rpx;
				text-align: center;

				text {
					font-size: 32rpx;
					font-family: PingFang SC;
					font-weight: 500;
					color: #FFFFFF;
					line-height: 70rpx;
				}
			}
		}

	}

	// 游客评价
	.evaluate .evaluateCenter .evaluateItem {
		display: inline-block;
	}

	.rightText {
		color: #4399FC;
		text-align: right;
	}

	.evaluate {
		.tip {
			padding: 30rpx;

			.left {
				font-size: 28rpx;
				font-weight: 500;
				color: #0c1317;
			}

			.right {
				color: #0f9efa;
				font-weight: bold;

				image {
					margin-left: 12rpx;
					width: 32rpx;
					height: 32rpx;
				}
			}
		}

		.evaluateCenter {
			overflow-x: auto;

			.evaluateItem {
				background-color: #FFFFFF;
				padding: 30rpx;
				border-radius: 20rpx;
				margin: 20rpx 30rpx 0 30rpx;

				.evaluateTip {
					.img {
						width: 74rpx;
						height: 74rpx;


						image {
							border-radius: 50%;
						}
					}

					.right {
						width: 100%;
						padding-left: 30rpx;

						.righttip {
							font-size: 26rpx;
							font-family: PingFang SC;
							font-weight: bold;
							color: #0C1317;
							padding-bottom: 10rpx;
						}

						.bottom {
							.star {
								display: flex;
								color: #fd9130;

								text {
									padding-right: 8rpx;
									font-size: 22rpx;
									color: #0f9efa;
									font-weight: bold;
								}
							}

							.time {
								color: #969899;
							}
						}
					}
				}

				.evaluateText {
					margin: 20rpx 0 0 0;
					letter-spacing: 1rpx;
					line-height: 40rpx;
				}

				.lineclamp3 {
					overflow: hidden;
					text-overflow: ellipsis;
					display: -webkit-box;
					-webkit-line-clamp: 3;
					-webkit-box-orient: vertical;
					white-space: normal;
				}

				.evaluateListImg {
					width: 100%;

					.evaluateListImgItem {
						width: 356rpx;
						height: 429rpx;
						margin-bottom: 20rpx;

						image {
							border-radius: 10rpx;
						}
					}
				}

				.bottomsp {
					width: 626rpx;
					height: auto;
					background: #F0F4FA;
					border-radius: 12rpx;

					.bottomspimg {
						image {
							width: 90rpx;
							height: 90rpx;
						}
					}

					.bottomspcont {
						padding-top: 10rpx;
						padding-left: 20rpx;

						.bottomspcont_name {
							font-size: 28rpx;
							font-family: PingFang SC;
							font-weight: 500;
							color: #333333;
						}

						.bottomspcont_name2 {
							font-size: 32rpx;
							font-family: DIN;
							font-weight: bold;
							color: #FF7315;
						}
					}
				}

				.bottomImg {
					display: flex;
					width: 100%;
					justify-content: space-between;
					margin-top: 40rpx;

					image {
						width: 38rpx;
						height: 38rpx;
					}

					text {
						font-size: 24rpx;
						font-family: PingFang SC;
						font-weight: 500;
						color: #999999;
					}
				}
			}
		}
	}

	.buttonn {
		width: 160rpx;
		height: 58rpx;
		border: 6rpx solid #1F7FFF;
		border-radius: 29rpx;
		text-align: center;

		text {
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #1F7FFF;
			line-height: 48rpx;
		}
	}
</style>