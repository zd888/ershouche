<template>
	<view class="feadtop">
		<view class="padding  usertop">
			<view class="flex align-center" style="padding-bottom: 10rpx;">
				<view class="u-m-r-10">
					<image :src="userInfo.avatar?userInfo.avatar:avatar"
						style="width: 100rpx;height: 100rpx;border-radius: 100rpx;" @click="goUserInfo"></image>
				</view>
				<view class="u-flex-1" v-if="!isLogin">
					<view class="flex" style="font-size: 32rpx;font-family: PingFang SC;color: #1D1D1D;">
						{{userInfo.userName}}
						<view class="rezheng" @click="gofabu()">
							<!-- 接口暂无 -->
							<text v-if="statusStauts==-1">未认证</text>
							<text v-else-if="statusStauts==0">待审核</text>
							<text v-else-if="statusStauts==1">已认证</text>
							<text v-else-if="statusStauts==2">已拒绝</text>
						</view>
					</view>
					<view class="flex align-center " v-if="XCXIsSelect!='否'">
						<view style="width: 17%;" @click="goNav('/my/detail/dongtai')">
							<view><text class="numtext">{{talkCount?talkCount:'0'}}</text><text class="textwenzi">动态</text></view>
						</view>
						<view style="width: 17%;margin: 0 20rpx;" @click="goNav('/my/feedback/myGuanZhu')">
							<view><text class="numtext">{{followCount?followCount:'0'}}</text><text class="textwenzi">关注</text></view>
						</view>
						<view style="width: 17%;" @click="goNav('/my/feedback/myFance')">
							<view><text class="numtext">{{fansCount?fansCount:'0'}}</text><text class="textwenzi">粉丝</text></view>
						</view>
					</view>
				</view>

				<view v-else class="text-xl u-p-l-20 text-bold" @click="goLogin('/pages/public/login')">
					登录
				</view>
			</view>
			<view class="vip flex padding">
				<view style="width: 80%;">
					<image src="../../static/images/my/vip.png" style="width:138rpx ; height:28rpx ;"></image>
					<view style="font-size: 26rpx;font-family: PingFang SC;font-weight: 500;color: #FFFFFF;"  v-if="isVip!=1">
						开通享受会员大特权
					</view>
					<view style="font-size: 26rpx;font-family: PingFang SC;font-weight: 500;color: #FFFFFF;" v-else>会员到期时间：{{vipEndTime}}</view>
				</view>
				<view class="button" @click="kaitongvip()" v-if="isVip!=1">
					<text class="buttonr">去开通</text>
				</view>
				<view class="button" v-else>
					<text class="buttonr">已开通</text>
				</view>

			</view>
			<view class="flex align-center margin-top-sm bg radius" style="height: 160rpx;" v-if="XCXIsSelect!='否'">
				<view class="text-center" style="width: 25%;" @click="goNav('/my/feedback/zuji')">
					<view style="font-size: 38rpx;font-family: DINPro;font-weight: bold;">{{zuji?zuji:'0'}}</view>
					<view style="font-size: 22rpx;font-family: PingFang SC;font-weight: 500;">我的足迹</view>
				</view>
				<view class="text-center" style="width: 25%;" @click="goNav('/my/feedback/shouCang')">
					<view style="font-size: 38rpx;font-family: DINPro;font-weight: bold;">{{shoiuchang?shoiuchang:'0'}}</view>
					<view style="font-size: 22rpx;font-family: PingFang SC;font-weight: 500;">我的收藏</view>
				</view>
				<view class="text-center" style="width: 25%;" @click="goNav('/my/feedback/Fangke')">
					<view style="font-size: 38rpx;font-family: DINPro;font-weight: bold;">{{browseUserCount?browseUserCount:'0'}}</view>
					<view style="font-size: 22rpx;font-family: PingFang SC;font-weight: 500;">我的访客</view>
				</view>
				<view class="text-center" style="width: 25%;" @click="goNav('/my/order/index')">
					<view style="font-size: 38rpx;font-family: DINPro;font-weight: bold;">{{releaseCount?releaseCount:'0'}}</view>
					<view style="font-size: 22rpx;font-family: PingFang SC;font-weight: 500;">我的发布</view>
				</view>
			</view>
		</view>

		<view class=" buycar" v-if="XCXIsSelect!='否'">
			<view class="buycar_con" @click="addcar()" style="padding: 30rpx 0;">
				<view class="text-lg   text-center ">
					<image src="../../static/images/my/addcar.png" style="width: 204rpx; height: 48rpx;"></image>
				</view>
				<view class="flex justify-center text-center padding-top-xs">
					<view>
						<view class="text-df" style="line-height: 50upx;">靠谱买家</view>
					</view>
					<view style="margin:0 60rpx">
						<view class="text-df" style="line-height: 50upx;">出价高卖</view>
					</view>
					<view>
						<view class="text-df" style="line-height: 50upx;">急速打款</view>
					</view>
				</view>
			</view>
		</view>
		<view class="padding">
			<view class="flex text-center flex-wrap bg radius" style="padding-top: 40rpx;">
				<!-- 邀请好友 -->
				<view class="padding-bottom" @click="goNav('/pages/my/invitationUser')" style="width: 25%;"
					v-if="XCXIsSelect!='否'">
					<image src="../../static/images/my/yaoqing.png" style="width:44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">邀请好友</view>
				</view>
				<!--  我的团队-->
				<view class="padding-bottom" @click="goNav('/my/team/team')" style="width: 25%;" v-if="zhiRate>0">
					<image src="../../static/images/my/anquan.png" style="width: 44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">我的团队</view>
				</view>
				<!-- 免费估值 -->
				<view class="padding-bottom" @click="goNav('/my/publish/guzhi')" style="width: 25%;"
					v-if="XCXIsSelect!='否'">
					<image src="../../static/images/my/mfgz.png" style="width: 44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">免费估值</view>
				</view>
				<view class="padding-bottom" @click="goNav('/my/publish/index')" style="width: 25%;"
					v-if="XCXIsSelect!='否'">
					<image src="../../static/images/my/guzhi.png" style="width: 44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">我的估值</view>
				</view>
				<!-- 我的认证 -->
				<view class="padding-bottom" v-if="XCXIsSelect!='否'"
					@click="statusType==''?goNav('/my/renzheng/check'):(statusType==1?goNav('/my/renzheng/index'):goNav('/my/renzheng/company'))"
					style="width: 25%;">
					<image src="../../static/images/my/renzheng.png" style="width: 44upx;height: 44upx;" mode="">
					</image>
					<view class="text-df" style="line-height: 50upx;">我的认证</view>
				</view>
				<!-- 联系客服 -->
				<view class="padding-bottom" @click="changekefu" style="width: 25%;" v-if="XCXIsSelect!='否'">
					<image src="../../static/images/my/kefu.png" style="width: 44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">联系客服</view>
				</view>
				<!-- 帮助中心 -->
				<view class="padding-bottom" @click="goNav('/my/setting/help')" style="width: 25%;">
					<image src="../../static/images/my/help.png" style="width: 44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">帮助中心</view>
				</view>
				<!-- 意见反馈 -->
				<view class="padding-bottom" @click="goNav('/my/feedback/index')" style="width: 25%;"
					v-if="XCXIsSelect!='否'">
					<image src="../../static/images/my/yijian.png" style="width: 44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">意见反馈</view>
				</view>
				<!-- 设置中心 -->
				<view class="padding-bottom" @click="goNav('/my/setting/index')" style="width: 25%;">
					<image src="../../static/images/my/login.png" style="width: 44upx;height: 44upx;" mode=""></image>
					<view class="text-df" style="line-height: 50upx;">设置中心</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		userInfo
	} from '@/apis/login.js'
	import {
		queryInsert
	} from '@/apis/my.js'
	export default {
		data() {
			return {
				avatar: '../../static/logo.png',
				isLogin: true,
				userName: '匿名',
				token: '',
				invitationCode: '', //邀请码
				renzheng: 0,
				userId: '',
				userInfo: {},
				talkCount: 0,
				followCount: 0,
				fansCount: 0,
				vipEndTime:'',
				
				statusType: '',
				isVip: 0, //未开通
				XCXIsSelect: '否',
				zhiRate: 0,
				zuji: 0,
				shoiuchang: 0,
				browseUserCount: 0,
				releaseCount: 0,

				statusStauts: -1,
			}
		},
		onLoad() {

		},
		onShow() {
			this.XCXIsSelect = this.$queue.getData('XCXIsSelect')
			this.token = uni.getStorageSync('token')
			this.userId = uni.getStorageSync('userId')
			console.log(this.userId)
			if (this.userId) {
				this.getUserInforz()
				this.isLogin = false
				this.getUserInfo()
				//获取认证信息
				// this.getQueryInsert()
				if (uni.getStorageSync('statusType')) {
					this.statusType = uni.getStorageSync('statusType')
				}

			} else {
				this.userInfo = {}
				this.isLogin = true
				this.userName = '匿名'
				this.isVip = 0
				this.followCount = 0
				this.fansCount = 0
				this.talkCount = 0
				this.vipEndTime= ''
				this.zuji = 0
				this.shoiuchang = 0
				this.fangke = 0
				this.releaseCount = 0
				this.browseUserCount = 0
				this.avatar = '../../static/logo.png'
			}

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
						uni.navigateTo({
							url: '/my/renzheng/check'
						})
						
						// uni.showModal({
						// 	title: '提示',
						// 	content: '请先进行认证',
						// 	complete(ret) {
						// 		if (ret.confirm) {
						// 			uni.navigateTo({
						// 				url: '/my/renzheng/check'
						// 			})

						// 		}
						// 	}
						// })
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
			getUserInforz() { //获取认证信息
				queryInsert().then(res => {
					if (res.code == 0 && res.data) {
						//认证类型 1:个人 2:企业
						uni.setStorageSync('statusType', res.data.userType)
						//认证状态 0:审核中 1:通过 2:拒绝
						uni.setStorageSync('statusStauts', res.data.status)
						this.statusStauts = res.data.status
						this.isSubmit = res.data.isSubmit
					} else {
						uni.removeStorageSync('statusType')
						uni.removeStorageSync('statusStauts')
						this.statusStauts = -1
					}
				})
			},
			kaitongvip() {
			
				if (uni.getStorageSync('token')) {
					uni.switchTab({
						url: '/pages/publish/index'
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
			//联系客服
			changekefu() {
				let kefu = this.$queue.getData('kefu'); // 用户端联系方式 1 手机号 2企业微信
				let kefuPhone = this.$queue.getData('kefuPhone');
				if (kefu == 1) {
					uni.makePhoneCall({
						phoneNumber: kefuPhone //仅为示例
					});
				} else if (kefu == 2) {
					// #ifdef MP-WEIXIN
					let that = this
					try {
						wx.openCustomerServiceChat({
							extInfo: {
								url: that.$queue.getData('kefuUrl')
							},
							corpId: that.$queue.getData('kefuAppId'),
							success(res) {},
							fail(res) {
								console.error(res)
							}
						})
					} catch (error) {
						console.error("catchcatch" + error)
						uni.showToast({
							title: '请更新至微信最新版本'
						});
					}
					// #endif
					// #ifndef MP-WEIXIN
					let url = this.$queue.getData('kefuUrl');
					if (url.indexOf('/pages/') !== -1 || url.indexOf('/my/') !== -1) {
						uni.navigateTo({
							url
						});
					} else {
						//#ifndef H5
						uni.navigateTo({
							url: '/pages/index/webView?url=' + url
						});
						//#endif
						//#ifdef H5
						window.location.href = url;
						//#endif
					}
					// #endif
				} else if (kefu == 3) {
					uni.navigateTo({
						url: '/my/kefu/kefu'
					});
				}
			},
			addcar() {
				if (uni.getStorageSync('token')) {
					if (this.statusStauts != 1) {
						this.gofabu()
						return
					}
					uni.navigateTo({
						url: '/my/publish/editor'
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
			goUserInfo() {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/my/userInfo/index'
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
			//获取用户认证信息查询
			getQueryInsert() {
				queryInsert().then(res => {
					if (res.code == 0 && res.data) {
						this.statusType = res.data.userType
						// uni.setStorageSync('userType',res.data.userType)
					} else {
						this.statusType = ''
					}
				})
			},
			goNav(e, type) {
				console.log(e)
				let that = this
				if (this.userId) {
					if (type == 1) { //需要认证（不论企业还是个人）
						if (uni.getStorageSync('statusType')) {
							if (uni.getStorageSync('statusStauts') == 1) {
								uni.navigateTo({
									url: e
								})
							} else if (uni.getStorageSync('statusStauts') == 2) {
								uni.showModal({
									title: '提示',
									content: '认证未通过，请重新认证',
									complete(ret) {
										if (ret.confirm) {
											uni.navigateTo({
												url: that.statusType == 1 ? '/my/renzheng/index' :
													'/my/renzheng/company'
											})
										}
									}
								})
							} else {
								uni.showToast({
									title: '认证审核中',
									icon: 'none'
								})
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
					} else if (type == 2) { //需要企业认证
						if (uni.getStorageSync('statusType') == 2) {
							if (uni.getStorageSync('statusStauts') == 1) {
								uni.navigateTo({
									url: e
								})
							} else if (uni.getStorageSync('statusStauts') == 0) {
								uni.showToast({
									title: '认证审核中',
									icon: 'none'
								})
							} else {
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
							}

						} else {
							uni.showToast({
								title: '企业认证用户可操作',
								icon: 'none'
							})
							// uni.showModal({
							// 	title:'提示',
							// 	content:'请先进行企业认证',
							// 	complete(ret) {
							// 		if(ret.confirm){
							// 			uni.navigateTo({
							// 				url:'/my/renzheng/check'
							// 			})
							// 		}
							// 	}
							// })
						}
					} else {
						uni.navigateTo({
							url: e
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
			goLogin(e) {
				uni.navigateTo({
					url: e
				})
			},
			//获取用户信息
			getUserInfo() {
				userInfo().then(res => {
					if (res.code == 0) {
						this.userInfo = res.data
						this.zhiRate = res.data.zhiRate ? res.data.zhiRate : '0'
						this.isVip = res.data.isVip
						this.invitationCode = res.data.invitationCode
						this.vipEndTime = res.data.vipEndTime
						this.talkCount = res.data.talkCount //动态
						this.followCount = res.data.followCount //关注
						this.fansCount = res.data.fansCount //粉丝
						this.zuji = res.data.browseCount //足迹
						this.shoiuchang = res.data.collectCount //收藏
						this.browseUserCount = res.data.browseUserCount //访客
						this.releaseCount = res.data.releaseCount //发布
						uni.setStorageSync('invitationCode', res.data.invitationCode)
						uni.setStorageSync('userName', res.data.userName)
						uni.setStorageSync('avatar', res.data.avatar)
						uni.setStorageSync('isVip', res.data.isVip)
						uni.setStorageSync('userInfo', res.data)
					} else if (res.code == 401) {
						uni.clearStorageSync();
					}
				})
			},
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #F2F6FC;
	}

	.bg {
		background: #FFFFFF;
	}

	.usertop {
		// width: 750rpx;
		// height: 500rpx;
		// background-image: url('../../static/images/my/bj.png');
		// background-repeat: no-repeat;
		// background-size: 100% 100%;
		background: linear-gradient(to bottom, #EAF9FF 30%, #F2F6FC 100%);

		.rezheng {
			width: 97rpx;
			height: 36rpx;
			background: linear-gradient(51deg, #1F7BFF, #21B2FF);
			border-radius: 18rpx;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #FFFFFF;
			text-align: center;
			line-height: 36rpx;
			margin-left: 10rpx;
			margin-top: 5rpx;
		}

		.vip {
			width: 686rpx;
			height: 135rpx;
			background: linear-gradient(90deg, #29241E 1%, #25211E 100%);
			border-radius: 24rpx;
			margin-top: 20rpx;

			.button {
				width: 130rpx;
				height: 64rpx;
				background: linear-gradient(130deg, #F8E9CC 0%, #FFC880 100%);
				border-radius: 32rpx;
				display: flex;
				align-items: center;
				justify-content: center;

				.buttonr {
					font-size: 28rpx;
					font-family: PingFang SC;
					font-weight: bold;
					color: #8A4C0B;
					
				}
			}
		}

		.numtext {
			font-size: 38rpx;
			font-family: PingFang SC;
			font-weight: 800;
			color: #1D1D1D;
			margin-right: 7rpx;
		}

		.textwenzi {
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 400;
			color: #1D1D1D;
		}
	}

	.buycar {
		width: 100%;
		height: 173rpx;

		.buycar_con {
			margin: 0 auto;
			width: 686rpx;
			// height: 173rpx;
			background: #F2F6FF;
			border: 1px solid #1F7FFF;
			border-radius: 12rpx;
		}
	}

	.feadtop {
		/* #ifdef APP */
		padding-top: 100rpx;
		/* #endif */
	}
</style>