<template>
	<view class="content">
		<!-- #ifndef H5 -->
		<!-- 导航栏 -->
		<!-- <u-navbar title="动态" :is-back="false" :background="background" title-color="#111224"></u-navbar> -->
		<!-- #endif -->

		<view class="bgjb"></view>

		<view class="tabs ">
			<view class="flex align-center">
				<view :class="tabIndex==index?'tabsSe':''" @tap="tabClick(index)"
					class="tabs-box-item flex justify-center align-center" v-for="(item,index) in tabBars" :key="index">
					{{item.name}}
				</view>
			</view>
			<view class="fdt" @click="bindfb">发动态</view>
		</view>

		<view class="list flex justify-center">
			<view class="list-box">
				<view class="list-box-item">
					<Head v-if="list.length>0" :list="list" :userId="userId" @click="clickItem"></Head>
					<!-- 暂无数据 -->
					<block v-if="list.length == 0">
						<view class="flex justify-center" style="margin-top: 200rpx;">
							<image src="../../static/images/empty.png" style="width: 387rpx;height: 341rpx;" mode="">
							</image>
						</view>
						<view class="flex justify-center"
							style="font-size: 32rpx;color: #999999;margin-top: 20rpx;font-weight: 700;">
							暂无数据
						</view>
					</block>
				</view>
			</view>
		</view>
		<!-- <view class="tui-tabs">
			<scroll-view id="tab-bar" scroll-with-animation class="tui-scroll-h" :scroll-x="true"
				:show-scrollbar="false">
				<view style="display: flex;">
					<view :key='index' v-for="(tab, index) in tabBars" :id="tab.id" :data-current="index"
						@tap="tabClick(index)">
						<view class="tui-tab-item-title" style="margin-left: 170upx;"
							:class="{ 'tui-tab-item-title-active': tabIndex == index }">{{ tab.name }}</view>
					</view>
				</view>
			</scroll-view>
		</view>

		<view style="background: #FFFFFF;height: 340upx;" v-if="bannerList.length > 0">
			<swiper class="swiper-container" :autoplay="true" :interval="5000" :circular="true" :indicator-dots="false"
				style="height: 260rpx;">
				<swiper-item class="swiper-wrapper" style="" v-for="(item, index3) in bannerList" :key="index3"
					@tap='toNavList(item.url)'>
					<image lazy-load='true' fade-show='true' :src="item.imageUrl" v-if="item"
						style="width: 100%;height: 260rpx;"></image>
				</swiper-item>
			</swiper>
		</view>

		<view>
			
		</view> -->
		<!-- <view class="part3" v-if="shangxianSelect != '否'" @click="bindfb">
			<image src="../../static/images/my/dtfb.png"></image>
		</view> -->

		<!-- 悬浮上拉 -->
		<!-- <view class="scroll_top" @tap="topScrollTap" v-bind:class="[scrollTop ? 'active' : '', '']"
				style="bottom: 56px;"><text class="iconfont icon-shangla"></text></view> -->

		<!-- 加载更多提示 -->
		<!-- 	<view class="s-col is-col-24" v-if="list.length > 0">
			<load-more :status="loadingType" :contentText="contentText"></load-more>
		</view> -->
		<!-- modal弹窗 -->
		<u-modal v-model="meShowModel" :content="meContent" :title="meTitle" :show-cancel-button='meShowCancel'
			@cancel="meHandleClose" @confirm="meHandleBtn" :confirm-text='meConfirmText'
			:cancel-text='meCancelText'></u-modal>
	</view>
</template>

<script>
	import Head from '../../components/Head.vue'
	import empty from '../../components/empty.vue'
	export default {
		components: {
			empty,
			Head
		},
		data() {
			return {
				background: {
					// backgroundImage: 'linear-gradient(90deg, #E4E7F8 0%, #F1E3F4 46%, #FDE1EF 100%)',
					backgroundColor: '#EBF9FF',

				},
				//弹窗
				meShowModel: false, //是否显示弹框
				meShowCancel: true, //是否显示取消按钮
				meTitle: '提示', //弹框标题
				meContent: '', //弹框内容
				meConfirmText: '确认', //确认按钮的文字
				meCancelText: '取消', //关闭按钮的文字
				meIndex: '', //弹窗的key
				shangxianSelect: '否',
				renzheng: 0,
				tabIndex: 0,
				tabBars: [{
						name: '全部',
						id: 'hot',
						state: '-1'
					}, {
						name: '关注',
						id: 'yule',
						state: '1'
					}

				],
				gambitId: 0,
				title: '',
				content: '',
				contentImg: '',
				bannerList: [],
				list: [],
				followUserId: 0,
				isEnable: '否',
				scrollTop: false,
				page: 1,
				size: 10,
				loadingType: 0,
				scrollTop: false,
				contentText: {
					contentdown: '上拉显示更多',
					contentrefresh: '正在加载...',
					contentnomore: '没有更多数据了'
				},
				totaSize: 0,
				userId: '',
				statusStauts: -1
			}
		},
		onLoad(d) {
			uni.showLoading({
				title: '加载中...'
			});

			let a = this.$queue.getData("isEnable")
			if (a) {
				this.isEnable = a;
			}

			uni.$on('click', (val) => {
				console.log("val______:" + val)
			});
		},
		onShow() {
			this.userId = this.$queue.getData('userId');
			// #ifdef MP-WEIXIN
			this.shangxianSelect = this.$queue.getData('shangxianSelect');
			// #endif
			// #ifndef MP-WEIXIN
			this.shangxianSelect = '是';
			// #endif
			this.page = 1;
			this.getList('');

			// this.getSelectBanner();
			this.getRenZheng();
		},
		onPageScroll: function(e) {
			this.scrollTop = e.scrollTop > 200;
		},
		methods: {
			gofabu() {
				if (this.userId) {
					if (uni.getStorageSync('statusType')) {
						console.log(uni.getStorageSync('statusType'))
						if (uni.getStorageSync('statusType') == 1) { //用户
							if (uni.getStorageSync('statusStauts') == 2) {
								uni.showModal({
									title: '提示',
									content: '认证未通过，请重新认证',
									complete(ret) {
										if (ret.confirm) {
											uni.navigateTo({
												url: '/my/renzheng/index'
											})
										}
									}
								})

							} else if (uni.getStorageSync('statusStauts') == 0) {
								uni.showToast({
									title: '认证审核中，请等待审核通过',
									icon: 'none',
									duration: 1200
								})
							}

						} else if (uni.getStorageSync('statusType') == 2) { //2 企业
							if (uni.getStorageSync('statusStauts') == 2) {
								uni.showModal({
									title: '提示',
									content: '认证未通过，请重新认证',
									complete(ret) {
										if (ret.confirm) {
											uni.navigateTo({
												url: '/my/renzheng/company'
											})
										}
									}
								})
							} else if (uni.getStorageSync('statusStauts') == 0) {
								uni.showToast({
									title: '认证审核中，请等待审核通过',
									icon: 'none',
									duration: 1200
								})
							}

						}

					} else {
						uni.showModal({
							title: '提示',
							content: '请先进行认证',
							complete(ret) {
								if (ret.confirm) {
									uni.navigateTo({
										url: '/my/renzheng/check'
									})
								}
							}
						})
					}

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
			getRenZheng() {
				this.$Request.get("/app/userCertification/queryInsert").then(res => {
					if (res.code == 0 && res.data) {
						//认证类型 1:个人 2:企业
						uni.setStorageSync('statusType', res.data.userType)
						//认证状态 0:审核中 1:通过 2:拒绝
						uni.setStorageSync('statusStauts', res.data.status)
						this.statusStauts = res.data.status

					} else {
						uni.removeStorageSync('statusType')
						uni.removeStorageSync('statusStauts')
						this.statusStauts = -1
					}
				});
			},
			/**
			 * @param {Object} url
			 */
			toNavList: function(url) {
				if (!url) return;
				let token = this.$queue.getData('token');
				if (token) {
					if (url.indexOf('/pages/') !== -1) {
						uni.navigateTo({
							url
						});
					} else {
						//#ifndef H5
						uni.navigateTo({
							url: '/pages/index/webview?url=' + url
						});
						//#endif
						//#ifdef H5
						window.location.href = url;
						//#endif
					}
				} else {
					this.goLoginInfo();
				}
			},
			getSelectBanner() {
				this.$Request.getT('/app/banner/selectBannerList?classify=6&state=1').then(res => {
					if (res.code == 0) {
						this.bannerList = res.data;
					}
				});
			},
			topScrollTap: function() {
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 300
				});
			},
			tabClick(e) {
				let token = this.$queue.getData('token');
				if (!token) {
					this.goLoginInfo();
					return;
				}
				this.tabIndex = e;
				this.page = 1;
				this.getList('');
			},
			clickItem: function(options) {
				let token = this.$queue.getData('token');
				if (!token) {
					this.goLoginInfo();
					return;
				}
				console.log(options)
				if (options.index == 0) {
					uni.navigateTo({
						url: '/my/detail/listDetails?trendsId=' + this.list[options.id].trendsId
					});
				} else if (options.index == 1) {
					let token = this.$queue.getData('token');
					if (token) {
						//去关注页面
						uni.navigateTo({
							url: '/my/gird/guanzhuDetail?userId=' + this.list[options.id].userId
						});

					} else {
						this.goLoginInfo();
					}
				} else if (options.index == 2) {
					let token = this.$queue.getData('token');
					if (token) {
						//去关注页面
						uni.navigateTo({
							url: '/my/userInfo/userInfo?userId=' + this.list[options.id].userId
						});

					} else {
						this.goLoginInfo();
					}
				} else if (options.index == 3) {
					this.saveLove(this.list[options.id].trendsId);
				} else if (options.index == 9) { //点击关注
					this.guanzhu1(this.list[options.id].userId);
				}else if (options.index == 8) {
					// uni.navigateTo({
					// 	url: '/my/releaseone/releaseone?trendsId=' + this.list[options.id].trendsId
					// })
					uni.navigateTo({
						url: '/pages/index/classify/detail?carId=' + this.list[options.id].carId
					})
				}
			},
			guanzhu1(userId) {
				this.$Request.postT('/app/scFollow/saveScFollow?byUserId=' + userId + '&type=2').then(res => {
					if (res.code == 0) {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
						this.page = 1;
						this.getList('');
					}
				});
			},
			//统一登录跳转
			goLoginInfo() {
				uni.navigateTo({
					url: '/pages/public/login'
				});
			},
			//确认
			meHandleBtn() {
				let that = this
				if (this.meIndex == 'm1') {
					uni.navigateTo({
						url: "/my/renzheng/index"
					});
				}
			},
			//取消
			meHandleClose() {
				let that = this
				if (this.meIndex == 'm1') {

				}
			},
			saveLove(trendsId) {

				let data = {
					trendsId: trendsId,
				}
				this.$Request.postT('/app/trendsLike/saveTrendsLike', data).then(res => {
					if (res.code == 0) {
						this.page = 1;
						this.getList('');
					} else {
						this.$queue.showToast(res.msg);
					}
				});
			},
			getList(type) {
				this.loadingType = 1;

				let userId = this.$queue.getData('userId') ? this.$queue.getData('userId') : '';
				let followUserId = 0;
				if (this.tabIndex != 0) {
					followUserId = userId;
				}
				let data = {}
				if (this.tabIndex != 0) {
					data = {
						page: this.page,
						limit: this.size,
						userId: userId
					}

				} else {
					data = {
						page: this.page,
						limit: this.size,
						byUserId: userId
					}
				}
				if (this.tabIndex != 0) {
					this.$Request.getT('/app/trends/getCareTrendsList', data).then(res => {
						if (res.code == 0) {
							this.totaSize = res.data.pages
							if (this.page == 1 || res.data) {
								this.list = [];
							}
							res.data.records.forEach(d => {
								if (d.trendsImage) {
									d.trendsImage = d.trendsImage.split(',');
								}
								if (d.titleImg) {
									let titleImg = d.titleImg.split(',');
									d.titleImg = titleImg[0]
								}
								this.list.push(d);
							})
							if (this.page < this.totaSize) { //还有下一页
								this.loadingType = 0;
							} else {
								this.loadingType = 2;
							}

						} else {
							this.loadingType = 2;
						}
						uni.hideLoading();
						if (type === 'refresh') {
							uni.stopPullDownRefresh();
						}
					});
				} else {
					this.$Request.getT('/app/trends/getTrendsList', data).then(res => {
						if (res.code == 0) {
							this.totaSize = res.data.pages
							if (this.page == 1 || res.data) {
								this.list = [];
							}
							res.data.records.forEach(d => {
								if (d.trendsImage) {
									d.trendsImage = d.trendsImage.split(',');
								}
								if (d.titleImg) {
									let titleImg = d.titleImg.split(',');
									d.titleImg = titleImg[0]
								}
								this.list.push(d);
							})
							if (this.page < this.totaSize) { //还有下一页
								this.loadingType = 0;
							} else {
								this.loadingType = 2;
							}

						} else {
							this.loadingType = 2;
						}
						uni.hideLoading();
						if (type === 'refresh') {
							uni.stopPullDownRefresh();
						}
					});
				}

			},
			//统一登录跳转
			goLoginInfo() {
				uni.navigateTo({
					url: '/pages/public/login'
				});
			},
			bindfb() {
				if (uni.getStorageSync('token')) {
					console.log(this.statusStauts)
					if (this.statusStauts != 1) {
						this.gofabu()
						return
					}
					uni.navigateTo({
						url: '/my/releaseone/releaseone'
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

			}
		},
		onReachBottom: function() {
			if (this.page < this.totaSize) {
				this.page = this.page + 1;
				this.getList('');
			}

		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getList('refresh');
			this.getSelectBanner();
		}
	}
</script>

<style lang="scss">
	.list {
		width: 100%;
		height: auto;
		margin-top: 120rpx;
		position: relative;
		z-index: 9;

		.list-box {
			width: 100%;
			height: 100%;

			.list-box-item {
				width: 100%;
				height: auto;
				// background-color: #FFFFFF;
				border-radius: 20rpx;
			}
		}
	}

	.lunbo {
		width: 100%;
		height: 200rpx;
		margin-top: 30rpx;

		.lunbo-box {
			width: 686rpx;
			height: 100%;
			border-radius: 20rpx;
		}
	}

	.tabsSe {
		// background-color: #AC75FE !important;
		color: #333333 !important;

		font-size: 40rpx !important;
		font-family: PingFang SC;
		font-weight: 800 !important;
	}

	.tabs {
		width: 100%;
		background: #EBF9FF;

		// background: linear-gradient(90deg, #E4E7F8 0%, #F1E3F4 46%, #FDE1EF 100%);
		position: fixed;
		/* #ifdef H5 */
		top: 0rpx;
		/* #endif */
		/* #ifndef H5 */
		top: 0;
		/* #endif */
		z-index: 99;
		padding: 35rpx 30rpx 20rpx;
		display: flex;
		// align-items: center;
		justify-content: space-between;
		z-index: 9999;

		.tabs-box-item {
			// width: 120rpx;
			height: 100%;
			// border-radius: 50rpx;
			// background: #FFFFFF;
			margin-right: 60rpx;
			font-size: 34rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #999999;
		}
	}

	.fdt {
		font-size: 30rpx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		background: linear-gradient(51deg, #1F7BFF, #21B2FF);
		// background: linear-gradient(0deg, #FF6F9C 0%, #FFA8C7 100%);
		width: 170rpx;
		height: 70rpx;
		border-radius: 55rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}


	// @import '../../static/less/index.less';
	// @import '../../static/css/index.css';

	page {
		background-color: #F7F7F7;
	}

	.bgjb {
		width: 100%;
		height: 450rpx;
		position: fixed;
		/* #ifdef H5 */
		top: 0rpx;
		/* #endif */
		/* #ifndef H5 */
		top: 0;
		/* #endif */
		z-index: 1;
		background: linear-gradient(to bottom, #EBF9FF 40%, #F7F7F7 60%, );
		// background-image: url('../../static/images/bgImg.png');
		// background-size: 100% 100%;
		// background: linear-gradient(to bottom, #e2d6feff, #F7F7F7);
		// background: linear-gradient(90deg, #E4E7F8 0%, #F1E3F4 46%, #FDE1EF 100%);
		// background:repeating-linear-gradient(red , yellow 0%, green 100%);
		padding-top: 40rpx;
		padding-bottom: 30rpx;
	}

	.tui-tabs {
		flex: 1;
		flex-direction: column;
		overflow: hidden;
		background-color: #FFFFFF;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		height: 100vh;
		/* #endif */
	}

	.tui-tab-item-title {
		color: #000000;
		font-size: 30rpx;
		height: 80rpx;
		line-height: 80rpx;
		flex-wrap: nowrap;
		white-space: nowrap;
	}

	.tui-tab-item-title-active {
		color: #AC75FE;
		font-size: 32upx;
		font-weight: bold;
		text-align: center;
	}



	.tui-scroll-h {
		width: 750rpx;
		height: 80rpx;
		background-color: #FFFFFF;
		flex-direction: row;
		/* #ifndef APP-PLUS */
		white-space: nowrap;
		/* #endif */
		/* #ifdef APP-PLUS */
		position: fixed;
		top: 0px;
		left: 0;
		z-index: 999;
		/* #endif */
		/* #ifdef H5 */
		position: fixed;
		top: 80rpx;
		left: 0;
		z-index: 999;
		/* #endif */
		/* #ifdef MP-WEIXIN */
		position: fixed;
		top: 0;
		left: 0;
		z-index: 999;
		/* #endif */
	}

	.content {
		width: 100%;
	}

	.part1 {
		position: relative;
	}

	.image_mengceng {
		width: 100%;
		height: 314rpx;
		opacity: 0.3;
		background: #000000;
		position: absolute;
		top: 0;
	}

	.content_bg {
		width: 100%;
		height: 314rpx;
	}

	.content_bg image {
		width: 100%;
		height: 314rpx;
	}

	.content_title {
		position: absolute;
		bottom: 25px;
		left: 20px;
		color: white;
	}

	.title {
		font-size: 38rpx;
		margin-bottom: 20rpx;
	}

	.content_title text {
		font-size: 24rpx;
		font-family: PingFang SC Medium, PingFang SC Medium-Medium;
		font-weight: 500;
		text-align: left;
		color: #ffffff;
	}

	.part3 {
		position: fixed;
		bottom: 150rpx;
		right: 10rpx;
		z-index: 999;
	}

	.part3 image {
		width: 200rpx;
		height: 200rpx;

	}
</style>