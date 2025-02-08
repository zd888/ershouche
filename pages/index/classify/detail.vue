<template>
	<view class="u-skeleton" style="padding-bottom:100upx;">
		<view class=" bg-white u-skeleton-fillet">
			<swiper class="swiper" :autoplay="true" interval="2000" duration="500" :circular="true"
				style="width: 100%;height: 500rpx;">
				<swiper-item @click="priveImgs(index,info.titleImg)" v-for="(item,index) in info.titleImg" :key='index'>
					<image :src="item" mode="aspectFill" style="width: 100%;height: 500rpx">
					</image>
				</swiper-item>
			</swiper>
		</view>
		<view class="padding bg-white u-skeleton-fillet">
			<view class="text-lg text-bold text-black">{{info.brandName}} {{info.modelsName}} {{info.carName}}</view>
			<view class="flex  align-center padding-tb-sm">
				<view class=" text-bold" style="color: #fd6416;font-size: 48rpx;">
					<span>{{info.money}}</span>
					<span style="font-size: 32rpx;">万</span>
				</view>
			</view>
		</view>
		<view class="xinxi">
			<view class="xinxi_N">
				<view class="padding-lr padding-tb-sm bg-white margin-top-sm u-skeleton-fillet"
					style="border-radius: 12rpx;">
					<view class="info">
						车辆信息
					</view>
					<view class="info-item flex  align-center " style="flex-wrap:wrap;">
						<view class="flex align-center  margin-tb-sm text-center" @click="goda"
							style="width: 100%; justify-content: space-evenly; ">
							<view class="names">档案</view>

							<view class="flex align-center justify-between" style="width: 85%;">
								<view class="text-sm" style="width: 25%;">
									<view>{{info.registerTime}}</view>
									<view style="color: #999999;">首次上牌</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view>{{info.kilometers}}万公里</view>
									<view style="color: #999999;">行驶里程</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view>{{infitem.environmentalstandards?infitem.environmentalstandards:'/'}}</view>
									<view style="color: #999999;">排放标准</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view>{{info.registerCity}}</view>
									<view style="color: #999999;">上牌地</view>
								</view>
							</view>
							<image src="../../../static/images/right1.png" style="width: 16rpx;" mode="widthFix">
							</image>
						</view>
						<view class="flex align-center margin-tb-sm text-center" @click="gocanshu"
							style="width: 100%; justify-content: space-evenly; ">
							<view class="names">参数</view>
							<view class="flex align-center justify-between" style="width: 85%;">
								<view class="text-sm" style="width: 25%;">
									<view>{{infitem.displacement?infitem.displacement:'/'}}</view>
									<view style="color: #999999;">排量</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view class="text-cut">{{infitem.geartype?infitem.geartype:'/'}}</view>
									<view style="color: #999999;">变速箱</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view>{{infitem.drivemode?infitem.drivemode:'/'}}</view>
									<view style="color: #999999;">驱动方式</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view>{{infitem.fueltype?infitem.fueltype:'/'}}</view>
									<view style="color: #999999;">燃料类型</view>
								</view>
							</view>
							<image src="../../../static/images/right1.png" style="width: 16rpx;" mode="widthFix">
							</image>
						</view>
						<view class="flex align-center margin-tb-sm text-center" @click="goda" v-if="info.drivingAuxiliary"
							style="width: 100%; justify-content: space-evenly; ">
							<view class="names">亮点</view>
							<view class="flex align-center justify-between" style="width: 85%;">
								<view class="text-sm" style="width: 25%;">
									<view>
										{{info.drivingAuxiliary.automaticparking}}
									</view>
									<view style="color: #999999;">自动驻车</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view >
										{{info.drivingAuxiliary.lanekeep}}
									</view>
									<view style="color: #999999;" class="text-cut">车道保持</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view >
										{{info.drivingAuxiliary.hillstartassist}}
									</view>
									<view style="color: #999999;">上坡辅助</view>
								</view>
								<view class="text-sm" style="width: 25%;">
									<view v-if="info.drivingAuxiliary.lanekeep">
										{{info.drivingAuxiliary.lanekeep}}
									</view>
									<view style="color: #999999;">车道保持</view>
								</view>
							</view>
							<image src="../../../static/images/right1.png" style="width: 16rpx;" mode="widthFix">
							</image>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="xinxi margin-tb-sm" v-if="info.userType==2" @click="goshop()">
			<view class="xinxi_N bg-white padding  flex " style="align-items: center;">
				<view style="width: 174rpx;margin-right: 20rpx;">
					<image :src="info.enterpriseAvatar" mode="widthFix" style="width: 174rpx;height: 135rpx;border-radius: 12rpx;"></image>
				</view>
				<view style="width: 446rpx; ">
					<view style="font-size: 32rpx;font-family: PingFang SC;font-weight: 500;color: #1A1A1A;">
						{{info.enterpriseName}}
					</view>
					<view class="margin-tb-xs" style="font-size: 26rpx;font-family: PingFang SC;color: #999999;">
						在售{{info.carCount}}辆</view>
					<view class="text-cut"
						style="font-size: 26rpx;font-family: PingFang SC;color: #999999;width: 448rpx;">
						地址：{{info.address}}</view>
				</view>
				<view>
					<image src="../../../static/images/right1.png" mode="widthFix" style="width: 16rpx;height: 28rpx;">
					</image>
				</view>
			</view>
		</view>


		<view class="container  " :style="info.userType==1?'margin-top:20rpx':''">
			<view class="containerN bg-white ">
				<view class="text-lg text-bold padding-bottom-sm" style="color: #1A1A1A;"> 车辆实拍</view>

				<image class="big-image" :src="bigImageUrl" mode="aspectFill" @click="priveImg(bigImageUrl)"></image>
				<view class="small-images-container">
					<image :src="item" class="small-image" mode="aspectFill" v-for="(item,index) in info.rotationImg"
						@click="priveImgs(index,info.rotationImg)" :key="index"></image>
				</view>
			</view>
		</view>
		<view class="tabber u-skeleton-fillet" v-if="userId!=info.userId">
			<view class="flex align-center " style="width: 40%;">
				<view @click="myServer()" v-if="info.isFollow!=1" style="width: 90rpx;text-align: center;">
					<image src="../../../static/images/index/shoucang.png" style="width: 48rpx;height: 48rpx;"></image>
					<view>收藏</view>
				</view>
				<view @click="myServer()" v-else style="width:120rpx;text-align: center;">
					<image src="../../../static/images/index/shoucangs.png" style="width: 48rpx;height: 48rpx;"></image>
					<view>已收藏</view>
				</view>

				<!-- #ifdef H5 -->
				<view @tap="showModal()" style="width: 150rpx;text-align: center;">
					<image src="../../../static/images/index/zhuanfa.png" style="width: 48rpx;height: 48rpx;"></image>
					<view>分享</view>
				</view>
				<!-- #endif -->
				<!-- #ifdef APP-PLUS -->
				<view @tap="showModal()" style="width: 150rpx;text-align: center;">
					<image src="../../../static/images/index/zhuanfa.png" style="width: 48rpx;height: 48rpx;"></image>
					<view>分享</view>
				</view>
				<!-- #endif -->

				<!-- #ifdef MP-WEIXIN -->
				<button open-type="share">
					<view style="width: 150rpx;text-align: center;">
						<image src="../../../static/images/index/zhuanfa.png" style="width: 48rpx;height: 48rpx;">
						</image>
						<view style="font-size: 28rpx;">分享</view>
					</view>
				</button>
				<!-- #endif -->

			</view>
			<view @click="callPhone(info.phone)" class="flex  align-center text-white phoneBtns">
				<text>联系卖家</text>
			</view>
		</view>

		<!-- #ifndef MP-WEIXIN -->
		<tki-qrcode ref="qrcode" :val="url" :size="200" background="#fff" foreground="#000" pdground="#000"
			:onval="true" :loadMake="true" @result="qrR" :show="false"></tki-qrcode>
		<view class="cu-modal" :class="modalName == 'Image' ? 'show' : ''" @tap="hideModal">
			<view class="cu-dialog" v-if="backgroundImage && erweimapath && haibaoShow" @tap="hideModal">
				<view class="bg-img">
					<wm-poster @success="posterSuccess" :imgSrc="backgroundImage" Referrer="" :QrSrc="erweimapath"
						:Title="tuiguang" :LineType="false"></wm-poster>
				</view>
			</view>
		</view>
		<!-- #endif -->

		<u-skeleton :loading="loading" :animation="true" bgColor="#FFF" style="height: 150vh;"></u-skeleton>
	</view>
</template>

<script>
	import {
		getDataById
	} from '@/apis/fabu.js'

	import {
		insertChatConversation
	} from '../../../apis/shop.js'
	import {
		typeList
	} from '../../../apis/pages.js'
	import tkiQrcode from '@/components/tki-qrcode/tki-qrcode.vue';
	import appShare from '@/utils/share.js';
	import wmPoster from '@/components/wm-poster/wm-posterorders.vue';
	import uQRCode from "../../../js_sdk/Sansnn-uQRCode/uqrcode.js"
	import _app from '../../../js_sdk/QuShe-SharerPoster/QS-SharePoster/app.js';
	import config from '../../../config.js'
	export default {
		data() {
			return {
				loading: true, // 是否显示骨架屏组件

				bigImageUrl: 'https://tcxx.xianmaxiong.com/file/uploadPath/2023/07/11/3ed2b0bd4c2bc86a1a4693aae8c883f1.png',
				smallImages: [
					'https://tcxx.xianmaxiong.com/file/uploadPath/2023/07/11/3ed2b0bd4c2bc86a1a4693aae8c883f1.png',
					'https://tcxx.xianmaxiong.com/file/uploadPath/2023/07/11/3ed2b0bd4c2bc86a1a4693aae8c883f1.png',
					'https://tcxx.xianmaxiong.com/file/uploadPath/2023/07/11/3ed2b0bd4c2bc86a1a4693aae8c883f1.png',
					'https://tcxx.xianmaxiong.com/file/uploadPath/2023/07/11/3ed2b0bd4c2bc86a1a4693aae8c883f1.png'
				],
				BannerList: [],
				tablist: [{
					title: '详情'
				}],
				count: 0,
				dataId: '',
				info: {},
				backgroundImage: '', //推广图片
				tuiguang: '', //推广文字
				invitationCode: '',

				haibaoImg: null,
				haibaoShow: false,
				modalName: '',
				url: '',
				erweimapath: '',
				show: false,
				userId: '',
				infitem:{}
			}
		},
		onShareAppMessage(res) {
			return {
				path: '/pages/index/classify/detail?invitation=' + this
					.invitationCode + '&carId=' + this.carId, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiguang,
				imageUrl: this.backgroundImage
			}
		},
		onShareTimeline(res) {
			return {
				path: '/pages/index/classify/detail?invitation=' + this
					.invitationCode + '&carId=' + this.carId, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiguang,
				imageUrl: this.backgroundImage
			}
		},
		onLoad(option) {
			// 分享
			// 获取邀请码保存到本地
			if (option.invitation) {
				this.$queue.setData('inviterCode', option.invitation);
			}
			// #ifdef MP-WEIXIN
			if (option.scene) {
				const scene = decodeURIComponent(option.scene);
				this.$queue.setData('inviterCode', scene.split(',')[0]);
			}
			// #endif
			this.carId = option.carId
			if (this.carId) {
				uni.showLoading({
					title: '加载中'
				})
			}

			// #ifndef H5
			typeList('25').then(res => {
				if (res.code == 0) {
					if (res.data && res.data.value) {
						this.url = res.data.value;
					}
				}
			})
			//#endif
			// #ifdef H5
			typeList('141').then(res => {
				if (res.code == 0) {
					if (res.data && res.data.value && res.data.value == '是') {
						typeList('25').then(ress => {
							if (ress.code === 0) {
								if (ress.data && ress.data.value) {
									this.url = ress.data.value;
								}
							}
						})
					} else {
						// this.url = this.$queue.publicYuMing() + '/?invitation=' + this.invitationCode;
						this.url = this.$queue.publicYuMing() + '/?invitation=' + this.invitationCode;
					}
				}
			})
			//#endif
		},
		onShow() {
			this.invitationCode = uni.getStorageSync('invitationCode') ? uni.getStorageSync('invitationCode') : '';
			this.userId = uni.getStorageSync('userId')
			if (this.carId) {
				this.getInfo();
			}

		},
		methods: {
			gocanshu() {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/my/brand/canshu?carId=' + this.carId
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
			goda() {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/my/brand/carDa?carId=' + this.carId
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
			goshop(item) {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/my/userInfo/userInfo?userId=' + this.info.userId
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
			//预览图片
			priveImgs(index, url) {
				uni.previewImage({
					current: index,
					urls: url
				})
			},
			priveImg(photoImg) {
				// uni.previewImage({
				// 	current: 0,
				// 	urls: url
				// })
				let imgsArray = [];
				imgsArray[0] = photoImg;
				uni.previewImage({
					current: 0,
					urls: imgsArray
				});
			},
			//活动页面
			huodong() {
				uni.navigateTo({
					url: '/my/buy/huodong'
				})
			},
			//去分享
			sharee() {

			},
			//收藏/取消收藏
			myServer() {
				if (uni.getStorageSync('token')) {
					let userId = uni.getStorageSync('userId')
					this.$Request.postT('/app/scFollow/saveScFollow?byUserId=' + userId + '&type=1&carId=' + this.carId)
						.then(res => {
							if (res.code == 0) {
								uni.showToast({
									title: res.msg,
									icon: 'none'
								})
								this.getInfo();
							}
						});
				} else {
					this.noLogin()
				}

			},
			noLogin() {
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
			},
			//拨打电话
			callPhone(phone) {
				if (uni.getStorageSync('token')) {
					uni.makePhoneCall({
						phoneNumber: phone
					})
				} else {
					this.noLogin()
				}
			},
			//获取服务详情
			getInfo() {
				let data = {
					carId: this.carId,
					userId: uni.getStorageSync('userId')
				}
				getDataById(data).then(res => {
					uni.hideLoading()
					if (res && res.code == 0) {
						this.info = res.data
						if (this.info.rotationImg) {
							let rotationImg = this.info.rotationImg.split(',')
							// console.log(rotationImg,rotationImg.length)
							this.bigImageUrl = rotationImg[0]
							let imgs = rotationImg.splice(1)
							// console.log(imgs,'====',imgs.length)
							this.info.rotationImg = imgs
							// console.log(this.info.rotationImg,imgs.length)
						}
						if (this.info.titleImg) {
							this.info.titleImg = this.info.titleImg.split(',')
							this.backgroundImage = this.info.titleImg[0]
						}
						this.tuiguang = this.info.brandName + ' ' + this.info.modelsName + ' ' + this.info.carName
						this.getTypelist()
						this.loading = false
					}
				})
			},
			getTypelist() {
				this.$Request.get("/app/carType/getCarTypeById?carTypeId=" + this.info.carTypeId).then(res => {
					uni.hideLoading();
					if (res && res.code == 0) {
						this.infitem = res.data
					}
				})
			},
			bindqie(index) {
				this.count = index
			},
			showModal() {
				if (!this.haibaoImg) {
					this.haibaoShow = true;
					this.$queue.showLoading('海报生成中...');
				} else {
					this.modalName = 'Image';
				}
			},
			qrR(path) {
				this.erweimapath = path;
			},
			posterSuccess(haibaoImg) {
				this.haibaoImg = haibaoImg;
				this.modalName = 'Image';
			},
			hideModal() {
				this.modalName = null;
			},
		}
	}
</script>

<style lang="less">
	button::after {
		border: none;
		background-color: none;
	}

	button {
		position: relative;
		display: block;
		margin-left: auto;
		margin-right: auto;
		padding-left: 0px;
		padding-right: 0px;
		box-sizing: border-box;
		text-decoration: none;
		line-height: 1.35;
		overflow: hidden;
		color: #666666;
		/* background-color: #fff; */
		background-color: rgba(255, 255, 255, 0) !important;
		width: 100%;
		height: 100%;
	}

	.info {
		font-size: 32rpx;
		font-weight: bold;
		color: #000000;
		margin-bottom: 10rpx;
	}

	.info-item {
		margin-bottom: 10rpx;

		.names {
			margin-top: 14rpx;
			font-size: 28rpx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #1A1A1A;
			flex: 1;
			text-align: left;
		}
	}

	.info-item-left {
		width: 50%;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		-o-text-overflow: ellipsis;
	}

	.info-item-right {
		width: 50%;
		color: #000000;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		-o-text-overflow: ellipsis;
	}

	.btn {
		padding: 4rpx 16rpx;
		color: #333333;
		background: #F2F2F2;
		border-radius: 8rpx;
		margin-right: 10rpx;
		margin-bottom: 10rpx;
	}

	.phoneBtn {
		display: flex;
		text-align: center;
		align-items: center;
		background: #fd6416;
		border-radius: 8rpx;
		padding: 18rpx 20rpx;
	}

	.phoneBtns {
		display: flex;
		text-align: center;
		align-items: center;
		width: 400rpx;
		height: 72rpx;
		background: linear-gradient(51deg, #1F7BFF, #21B2FF);
		border-radius: 36rpx;
		justify-content: center;
	}

	.tabber {
		width: 750upx;
		/* height: 98upx; */
		background: #ffffff;
		display: flex;
		align-items: center;
		position: fixed;
		bottom: 0;
		right: 0;
		left: 0;
		justify-content: space-around;
		padding: 20rpx 50rpx 30rpx 30rpx;
	}

	.tabColor {
		color: #FF6800;
	}

	// 企业样式
	.xinxi {
		width: 100%;

		.xinxi_N {
			width: 95%;
			margin: 0 auto;
			border-radius: 12rpx;
		}

	}


	.container {
		width: 100%;
		padding-bottom: 50rpx;
	}

	.containerN {
		width: 95%;
		margin: 0 auto;
		border-radius: 12rpx;
		padding: 20rpx;
	}

	.big-image {
		width: 100%;
		height: 487rpx;
		border-radius: 12rpx;
	}

	.small-images-container {
		width: 100%;
		height: auto;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}

	.small-image {
		width: 320rpx;
		height: 219rpx;
		margin: 20rpx 0;
		border-radius: 12rpx;
	}
</style>