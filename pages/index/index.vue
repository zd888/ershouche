<template>
	<view class="">
		<view class="padding flex justify-between align-center  headsr">
			<view class="flex align-center">
				<view class=" text-df text-bold text-black" @tap="goNav('/my/citys/citys')">{{city}}</view>
				<image src="../../static/images/index/xiala.png" style="width: 20rpx;height: 12rpx;"
					class="margin-lr-sm">
				</image>
			</view>
			<view class="searchbox" @click="gotoSearch()">
				<image
					src="https://ershouche.xianmaxiong.com/file/uploadPath/2024/01/16/2b8ed866dab91a4433c57b7c8cacf244.png"
					style="width: 34rpx;height:34rpx;"></image>
				<view class="margin-left-xs" style="color: #666666;font-size: 26rpx;">搜索你想买的车</view>
			</view>
		</view>
		<!-- 轮播图 -->
		<view class="padding-lr  headapp">
			<swiper class="swiper" :autoplay="true" interval="2000" duration="500" :circular="true"
				style="width: 100%;height: 180rpx;">
				<swiper-item v-for="(item,index) in BannerList" :key='index' @tap="goNav(item.url)">
					<image :src="item.imageUrl" mode="scaleToFill" style="width: 100%;height: 180rpx;"></image>
				</swiper-item>
			</swiper>
		</view>
		<!-- 金刚区 -->
		<view class="margin-bottom-sm" style="color: #BFBFCB;">
			<view class="category" v-if="gridData.length>0">
				<swiper class="swiper" duration="300" :style="{ height: categoryHeight }">
					<swiper-item v-for="(nav, index5) in gridData" :key="index5">
						<view class="category-list">
							<view class="icon" v-for="(item,ind) in nav" :key="ind" @tap="goNav(item.url)">
								<image mode="widthFix" :src="item.imageUrl" style="height: 54rpx;width: 54rpx">
								</image>
								<view
									style="font-size: 24rpx;font-family: PingFang SC;font-weight: 500;color: #333333;">
									{{ item.name }}
								</view>
							</view>
						</view>
					</swiper-item>
				</swiper>
				<!-- <view class="dots">
							<view v-for="(page, pageindex) in gridData" :key="pageindex"
								:class="{ active: pageindex == currentPageindex }"></view>
						</view> -->
			</view>
		</view>

		<!-- 类型 -->
		<view class="announcement" >
			<!-- 品牌 -->
			<view class="announcementbox">
				<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120" lower-threshold="10"
					@touchmove.stop @scrolltolower="scrolltolowerss">
					<view class=" scroll-view-item_H" v-for="(item,index) in brandlist" :key="index"
						@click="goCarlist(item)">
						<image mode="widthFix" :src="item.logo" style="height: 54rpx;width: 54rpx">
						</image>
						<view class="tt">
							{{ item.name }}
						</view>
					</view>
				</scroll-view>

			</view>
			<view class="announcementbox">
				<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="0" @touchmove.stop
					@scrolltolower="scrolltolowerprice">
					<view class=" scroll-view-item_Price" v-for="(item,index) in brandMoney" :key="index"
						@click="tabBanerMonry(item,index)" :class="index==moneyIndex?'scroll-view-item_PriceAct':''">
						<view style="font-size: 24rpx;font-family: PingFang SC;font-weight: 500;color: #333333;">
							<text v-if="item.maxMoney==0">{{item.minMoney}}万以上</text>
							<text v-if="item.minMoney==0">{{item.maxMoney}}万以下</text>
							<text v-if="item.minMoney!=0&&item.maxMoney!=0">{{item.minMoney}}-{{item.maxMoney}}万</text>
						</view>
					</view>
				</scroll-view>
			</view>
			<view class="announcementbox">
				<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="0" @touchmove.stop>
					<view class="scroll-view-item_labe" v-for="(item,index) in labelist" :key="index"
						@click="tabBanerLabe(item,index)" :class="index==labesIndex?'scroll-view-item_labeAct':''">
						<view style="font-size: 24rpx;font-family: PingFang SC;font-weight: 500;color: #333333;">
							{{ item.value }}
						</view>
					</view>
				</scroll-view>
			</view>
		</view>

		<!-- 展厅活动 -->
		<view class=" padding flex " v-if="gglist.length!=0">
			<view class="margin-right" @tap="goNav(gglist[0].url)">
				<image :src="gglist[0].imageUrl" mode="widthFix" style="width: 333rpx; height: 187rpx;"></image>
			</view>
			<view @tap="goNav(gglist[1].url)">
				<image :src="gglist[1].imageUrl" mode="widthFix" style="width: 333rpx; height: 187rpx;"></image>
			</view>
		</view>

		<!-- tab切换 -->
		<view class="tabberN">
			<view class="end-title">
				<view v-for="(item,index) in items" :key="index" :class="itemsIndex == index?'btna':''"
					@tap="changee(index)">
					{{item}}
					<image class="imgeswx" v-if="itemsIndex == index"
						src="https://ershouche.xianmaxiong.com/file/uploadPath/2024/01/15/911591969a20179deb564fbbf6620b4a.png"
						style="width: 27rpx;height: 12rpx;"></image>
				</view>
			</view>


			<view class="end-cont ">
				<view class="  ">
					<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120" lower-threshold="10"
						@scrolltolower="scrolltolowerday">
						<view class="scroll-view-item_H" v-for="(item,index) in daylist" :key="index"
							@click="clickItem(item)">
							<image :src="item.titleImg" mode="aspectFill"
								style="width: 202rpx; height: 150rpx; border-radius: 20rpx;">
							</image>
							<view class="names text-cut">{{item.brandName}} {{item.modelsName}} {{item.carName}}</view>
							<view style="font-size: 32rpx;font-family: PingFang SC;font-weight: 800;color: #FF7315;">
								{{item.money}}万
							</view>
						</view>
						<view class="text-center margin-top" v-if="daylist.length==0">
							<image src="../../static/images/empty.png" mode="widthFix" style="width: 200rpx;"></image>
							<view class="margin-top-sm" style="color: #999999;">暂无数据</view>
						</view>
					</scroll-view>

				</view>
			</view>
		</view>

		<view class="flex align-center" style="padding: 0 20rpx;position: relative;">

			<!-- <view class="" @click="tuijian" :class="!istj?'acttj':'tuij'">推荐</view> -->
			<u-dropdown :menu-icon='menuicon' ref="uDropdown" @open="open" @close="close" menu-icon-size="18">
				<u-dropdown-item v-model="value1" :title="mrtitle">
					<view class="slot-content">
						<view class=" flex flex-wrap padding">
							<view class="listpp" v-for="(item,index) in mrlist" :key="index"
								:class="mrIndex==index?'listppAct':''" @click="changeTj(item,index)">
								{{item.label}}
							</view>
						</view>
					</view>
				</u-dropdown-item>
				<u-dropdown-item v-model="value2" :title="titlepp"> </u-dropdown-item>
				<u-dropdown-item v-model="value3" :title="pricetit">
					<view class="slot-content padding">
						<view class="flex flex-wrap">
							<view class="listpp" :class="MoneyIndex==-1?'listppAct':''" @click="bindPrice('',-1)">
								不限价格
							</view>
							<view class="listpp" v-for="(item,index) in brandMoney" :key="index"
								@click="bindPrice(item,index)" :class="MoneyIndex==index?'listppAct':''">
								<text v-if="item.maxMoney==0">{{item.minMoney}}万以上</text>
								<text v-if="item.minMoney==0">{{item.maxMoney}}万以下</text>
								<text
									v-if="item.minMoney!=0&&item.maxMoney!=0">{{item.minMoney}}-{{item.maxMoney}}万</text>
							</view>
						</view>

						<view v-if="isPrice">
							<slider-range :value="rangeValue" :min="rangeMin" :max="rangMax" :step="1" :bar-height="10"
								:block-size="20" background-color="#E0E3EA" active-color="#2096FF" :format="format"
								:decorationVisible="false" @change="handleRangeChange"></slider-range>
						</view>
						<view class="btn" @click="changePrice">确定</view>
					</view>
				</u-dropdown-item>
				<u-dropdown-item v-model="value4" :title="carname">
					<view class="slot-content">
						<view class="padding">
							<view>
								<view>车龄(年)</view>
								<slider-range :value="rangeCarage" :min="carAgeMin" :max="carAgeMax" :step="1"
									:bar-height="10" :block-size="20" background-color="#E0E3EA" active-color="#2096FF"
									:format="format" :decorationVisible="false"
									@change="handleRangeChangecarAge"></slider-range>
							</view>
							<view>
								<view>里程(万公里)</view>
								<slider-range :value="rangeValuelc" :min="lcMin" :max="lcMax" :step="1" :bar-height="10"
									:block-size="20" background-color="#E0E3EA" active-color="#2096FF" :format="format"
									:decorationVisible="false" @change="handleRangeChangelc"></slider-range>
							</view>
							<view class="flex ">
								<view class="btn flex-sub" @click="chongzhi">重置</view>
								<view class="btn flex-sub" @click="bindtab">确定</view>
							</view>
						</view>
					</view>
				</u-dropdown-item>
			</u-dropdown>
		</view>

		<view class="labelsx">
			<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="0">
				<view class="scroll-view-item_labe" v-for="(item,index) in labelist" :key="index"
					@click="bindlabe(item,index)" :class="item.isTrue&&labeIndex==index?'scroll-view-item_labeAct':''">
					<view style="font-size: 24rpx;font-family: PingFang SC;font-weight: 500;color: #333333;">
						{{ item.value }}
					</view>
				</view>
			</scroll-view>
		</view>

		<view class="flowListStyle" v-if="orderlist.length!=0">
			<view class="flowListStyle-box">
				<orderlist :orderlist="orderlist" @clickItem="clickItem"></orderlist>
				<u-loadmore v-if="orderlist.length>0" :status="status" />
			</view>
		</view>
		<empty height="0" style="padding-top: 500rpx;" v-if="orderlist.length==0" />
	</view>
</template>

<script>
	import {
		getClassList,
		getClassLists,
		getDataList,
		getBannerList,
		getNoticeList,
		getSelectCity
	} from '@/apis/index.js'

	import {
		typeList
	} from '@/apis/pages.js'
	import empty from '@/components/empty.vue'
	import orderlist from '@/components/orderlist.vue'
	import SliderRange from '@/components/primewind-sliderrange/index.vue'
	export default {
		components: {
			empty,
			orderlist,
			SliderRange
		},
		data() {
			return {
				menuicon: 'arrow-down-fill',
				btnnum: 0,
				ggMsg:[],
				ggUrl:'',
				items: ["低价专区", "每日上新"],
				itemsIndex: 0,
				currentPageindex: 0,
				categoryHeight: '240rpx',
				tabbarList: this.$store.state.list,
				enable: true,
				gongao: [],
				city: '',
				BannerList: [],
				gridData: [],

				brandlist: [],
				titlepp: '品牌',
				counts: 0,
				brandIndex: -1,

				page: 1,
				limit: 10,
				brandMoney: [],
				pricetit: '价格',
				MoneyIndex: -1,
				minMoney: '',
				maxMoney: '',
				rangeMin: 5,
				rangMax: 200,
				rangeValue: [0, 100],

				labelist: [],
				labeIndex: -1,


				carAge: [], //车龄
				carAgeMin: '',
				carAgeMax: '',
				rangeCarage: [],
				carname: '车龄/里程',
				pageday: 1,
				count: 0,
				daylist: [], //，每日上新

				list: [],
				value1: 1,
				value2: 2,
				value3: 1,
				value4: 2,

				current: 0,
				classId: '',
				orderlist: [],
				minCarYear: '',
				maxCarYear: '',
				brandName: '',

				leftList: [],
				rightList: [],
				page: 1,
				limit: 10,
				count: 1,

				lng: '',
				lat: '',
				status: 'loadmore',
				showModal: true,
				arr: [],
				myId: '',
				tuiguang: '', //推广文案
				backgroundImage: '', //推广背景图
				invitationCode: '',
				gglist: [],
				istj: false,
				label: '',
				moneyIndex: -1,

				maxMoneyban: '',
				minMoneyban: '',
				labesIndex: -1,
				labesban: '',
				mrtitle: '默认排序',
				mrlist: [{
					label: '默认排序',
					value: 1,
				}, {
					label: '价格最低',
					value: 2,
				}, {
					label: '价格最高',
					value: 3,
				}, {
					label: '里程最少',
					value: 4,
				}, {
					label: '车龄最短',
					value: 5,
				}],
				mrIndex: 0,
				moneySort: '', //:价格最高 2:价格最低
				kilometersSort: '', //:里程最多 2:里程最少
				carYearSort: '', //车龄最长 2车龄最短
				keyword: '',
				rangeValuelc: [],
				lcMax: '',
				lcMin: '',
				minKilometers: '',
				maxKilometers: '',
				type: '',
				carlist: '',
				isPrice: false
			}
		},
		onShareAppMessage(res) {
			return {
				path: '/pages/index/index?invitation=' + this
					.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiguang,
				imageUrl: this.backgroundImage
			}
		},
		onShareTimeline(res) {
			return {
				path: '/pages/index/index?invitation=' + this
					.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiguang,
				imageUrl: this.backgroundImage
			}
		},
		onHide() {
			this.enable = false
		},
		onPullDownRefresh() {
			this.page = 1
			this.getFabuList()
		},
		onReachBottom() {
			if (this.page < this.count) {
				this.page += 1
				this.status = 'loading'
				this.getFabuList()
			} else {
				this.status = 'nomore'
			}
		},
		onLoad(e) {
			this.enable = true

			// this.current = 0
			//获取分类列表
			this.getClassLists();
			// this.getClassListof();
			//banner图
			this.getBannerImage()
			// 首页广告位
			this.getguang()
			//获取品牌
			this.getbrand()
			// 获取价格分类
			this.getMoney()
			// 获取标签
			this.getlabel()
			// 获取车龄
			this.getCarage()

			// 获取里程
			this.getlc()

			//公告列表
			// this.getGonggaoList()
			this.myId = uni.getStorageSync('userId')
			// 分享
			// 获取邀请码保存到本地
			if (e.invitation) {
				this.$queue.setData('inviterCode', e.invitation);
			}
			// #ifdef MP-WEIXIN
			if (e.scene) {
				const scene = decodeURIComponent(e.scene);
				this.$queue.setData('inviterCode', scene.split(',')[0]);
			}
			// #endif
		},
		onShow() {
			this.invitationCode = uni.getStorageSync('invitationCode') ? uni.getStorageSync('invitationCode') : '';
			//推广文案
			typeList('276').then(res => {
				if (res.code == 0) {
					if (res.data && res.data.value) {
						this.tuiguang = res.data.value;
					}
				}
			})
			//推广图片
			typeList('289').then(res => {
				if (res.code == 0) {
					if (res.data && res.data.value) {
						this.backgroundImage = res.data.value;
					}
				}
			})

			//获取城市
			let that = this
			if (uni.getStorageSync('city') && uni.getStorageSync('lat')) {
				that.lat = uni.getStorageSync('lat')
				that.lng = uni.getStorageSync('lng')
				that.city = uni.getStorageSync('city')


				if (uni.getStorageSync('carlist')) {
					let carlist = uni.getStorageSync('carlist')
					that.carlist = carlist.toString()
					uni.removeStorageSync('carlist')
				} else {
					that.carlist = ''
				}

				if (uni.getStorageSync('brandName')) {
					that.titlepp = uni.getStorageSync('brandName')
					uni.removeStorageSync('brandName')
				} else {
					that.titlepp = '品牌'
				}

				that.getFabuList();
				// console.log(carlist.toString())

				that.getdaylist()
			} else {
				uni.getLocation({
					type: 'gcj02', //wgs84  gcj02
					success: function(res) {
						//根据定位的经纬度获取城市名称
						that.lat = res.latitude
						that.lng = res.longitude
						that.selectCity(res.longitude, res.latitude);
					},
					fail: function() {
						console.log('获取地址失败')
					}
				})
			}

			// #ifdef MP-WEIXIN
			//订阅
			if (that.myId) {
				typeList('294').then(res => {
					if (res.code == 0) {
						if (res.data && res.data.value) {
							that.arr.push(res.data.value)
						}
					}
				})
				typeList('292').then(res => {
					if (res.code == 0) {
						if (res.data && res.data.value) {
							that.arr.push(res.data.value)
						}
					}
				})
				if (that.showModal) {
					that.openMsg()
				}
			}
			// #endif

		},
		methods: {
			start(e) {
				console.log(e, 'start')
			},
			stop(e) {
				console.log(e, 'stop')
			},
			changeTj(e, index) {
				console.log(e, '8888888888888')
				if (e.value == 2) {
					this.moneySort = 2
					this.kilometersSort = ''
					this.carYearSort = ''
				} else if (e.value == 3) {
					this.moneySort = 1
					this.kilometersSort = ''
					this.carYearSort = ''
				} else if (e.value == 4) {
					this.kilometersSort = 2
					this.moneySort = ''
					this.carYearSort = ''
				} else if (e.value == 5) {
					this.carYearSort = 2
					this.kilometersSort = ''
					this.moneySort = ''
				} else if (e.value == 1) {
					this.carYearSort = ''
					this.kilometersSort = ''
					this.moneySort = ''
				}
				this.page = 1
				this.mrIndex = index
				this.getFabuList();
				this.$refs.uDropdown.close();
				// moneySort: '', //:价格最高 2:价格最低
				// kilometersSort:'', //:里程最多 2:里程最少
				// carYearSort: '', //车龄最长 2车龄最短
			},
			tabBanerLabe(item, index) {

				if (uni.getStorageSync('token')) {
					this.labesban = item.value
					// this.labesIndex = index
					uni.navigateTo({
						url: '/my/buy/ershouche?labes=' + this.labesban + '&isFirst=1&labelIndex=' + index
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
			tabBanerMonry(item, index) {
				if (uni.getStorageSync('token')) {
					// this.moneyIndex = index
					this.maxMoneyban = item.maxMoney
					this.minMoneyban = item.minMoney
					uni.navigateTo({
						url: '/my/buy/ershouche?maxMoney=' + this.maxMoneyban +
							'&minMoney=' + this.minMoneyban + '&isFirst=1'
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
			bindlabe(ite, ind) {
				this.labeIndex = ind

				this.labelist.map((item, index) => {
					if (index == ind) {
						item.isTrue = !item.isTrue
					} else {
						item.isTrue = false
					}
				})
				if (this.labelist[ind].isTrue) {
					this.label = ite.value
				} else {
					this.label = ''
				}
				console.log(this.label)
				this.page = 1
				this.getFabuList();

			},
			tuijian() {
				console.log('11')
				this.istj = !this.istj
			},

			open(index) {
				console.log('open', index)
				if (index == 1) {
					this.$refs.uDropdown.close();
					// this.$refs.uDropdown.highlight();
					uni.navigateTo({
						url: '/my/brand/index?type=1'
					})
				} else {
					// this.keyword = ''
					// this.getbrand()
					// 展开某个下来菜单时，先关闭原来的其他菜单的高亮
					// 同时内部会自动给当前展开项进行高亮
					this.$refs.uDropdown.highlight();
				}

			},
			close(index) {
				console.log('close', index)
				// 关闭的时候，给当前项加上高亮
				// 当然，您也可以通过监听dropdown-item的@change事件进行处理
				this.$refs.uDropdown.highlight(index);
			},
			goSwitchTab(url) {
				uni.switchTab({
					url: url
				})
			},
			GoKeFu(){
				if(!this.ggUrl){
					return;
				}
				uni.makePhoneCall({
					phoneNumber: this.ggUrl
				})
				
			},
			goNav(url) {
				if (uni.getStorageSync('token')) {
					if(!url){
						return;
					}
					if (url.indexOf('/my/') !== -1) {
						uni.navigateTo({
							url
						})
					} else if (url.indexOf('/pages/') !== -1) {
						//#ifndef H5
						uni.switchTab({
							url: url
						});
						//#endif
						//#ifdef H5
						window.location.href = url;
						//#endif	
					}else{
						//#ifndef H5
						uni.navigateTo({
							url: '/pages/index/webView?url=' + url
						});
						//#endif
						//#ifdef H5
						window.location.href = url;
						//#endif
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
			//搜索页面
			gotoSearch() {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/pages/index/search/index'
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
			//根据经纬度获取城市地址
			selectCity(longitude, latitude) {
				let that = this
				let data = {
					lat: latitude,
					lng: longitude
				}
				getSelectCity(data).then(res => {
					if (res.code == 0) {
						//修改当前城市
						uni.setStorageSync('lat', latitude)
						uni.setStorageSync('lng', longitude)
						uni.setStorageSync('city', res.data.city)
						that.city = res.data.city
						// that.getShopLists()
						that.getFabuList();
						that.getdaylist()
					}
				})
			},
			//获取公告
			getGonggaoList() {
				getNoticeList().then(res => {
					if (res.code == 0) {
						let arr = []
						res.data.records.map(item => {
							arr.push(item.content)
						})
						this.gongao = arr
					}
				})
			},
			//获取banner图
			getBannerImage() {
				let data = {
					classify: 1

				}
				getBannerList(data).then(res => {
					if (res && res.code == 0) {
						this.BannerList = res.data
					}
				})
			},

			getguang() {
				let data = {
					classify: 7,
				}
				getClassList(data).then(res => {
					if (res && res.code == 0) {
						this.gglist = res.data
					}
				})
			},
			//获取分类列表
			getClassLists() {
				let data = {
					classify: 2,
					type: 1
				}
				getClassList(data).then(res => {
					if (res && res.code == 0) {
						if (res.data.length > 0) {
							var datanew = this.chunk(res.data, 8)
							this.gridData = datanew;
							if (res.data.length > 5) {
								this.categoryHeight = "240rpx"
							} else {
								this.categoryHeight = "120rpx"
							}
						} else {
							var datanew = this.chunk(this.gridData, 8)
							this.gridData = datanew;
						}
					}
				})
			},
			// 传进数组和指定个数，进行拆分
			chunk: function(array, size) {
				//获取数组的长度，如果你传入的不是数组，那么获取到的就是undefined
				const length = array.length
				//判断不是数组，或者size没有设置，size小于1，就返回空数组
				if (!length || !size || size < 1) {
					return []
				}
				//核心部分
				let index = 0 //用来表示切割元素的范围start
				let resIndex = 0 //用来递增表示输出数组的下标

				//根据length和size算出输出数组的长度，并且创建它。
				let result = new Array(Math.ceil(length / size))
				//进行循环
				while (index < length) {
					//循环过程中设置result[0]和result[1]的值。该值根据array.slice切割得到。
					result[resIndex++] = array.slice(index, (index += size))
				}
				//输出新数组
				return result
			},
			scroll(e) {
				// console.log(e,'1111')
			},
			scrolltolowerss(e) {
				console.log(e, '8888888')
				this.page += 1
				this.getbrand()
			},
			scrolltolowerprice(e) {
				console.log(e, '99999')
			},
			// 获取品牌
			getbrand() {
				let data = {
					page: this.page,
					limit: this.limit,
					name: this.keyword
				}
				this.$Request.get("/app/brand/getBrandList", data).then(res => {
					uni.hideLoading();
					if (res.code === 0) {
						this.counts = res.data.pages
						if (this.page == 1) {
							this.brandlist = res.data.records
						} else {
							this.brandlist = [...this.brandlist, ...res.data.records]
						}

					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
					}
				});
			},

			format(val) {
				return val
				// return val + '%'
			},
			handleRangeChange(e) {
				console.log(e)
				this.rangeValue = e
			},
			changePrice() {

				if (this.isPrice) {
					this.pricetit = this.rangeValue[0] + '-' + this.rangeValue[1] + '万'
					this.minMoney = this.rangeValue[0]
					this.maxMoney = this.rangeValue[1]
				}
				this.page = 1
				this.getFabuList();
				this.$refs.uDropdown.close();
			},
			bindPrice(item, index) {
				this.MoneyIndex = index
				if (index != -1) {
					if (item.maxMoney == 0) {
						this.pricetit = item.minMoney + '万以上'
						this.minMoney = item.minMoney
						this.maxMoney = item.maxMoney
						this.isPrice = false
					} else if (item.minMoney == 0) {
						this.pricetit = item.maxMoney + '万以下'
						this.minMoney = item.minMoney
						this.maxMoney = item.maxMoney
						this.isPrice = false
					} else if (item.minMoney != 0 && item.maxMoney != 0) {
						this.pricetit = item.minMoney + '-' + item.maxMoney + '万'
						this.minMoney = item.minMoney
						this.maxMoney = item.maxMoney
						this.isPrice = true
						this.rangeValue = [item.minMoney, item.maxMoney]
					}

				} else {
					this.isPrice = false
					this.minMoney = ''
					this.maxMoney = ''
					this.pricetit = '价格'
				}
			},
			// 获取价格分类
			getMoney() {
				this.$Request.get("/app/screenMoney/getScreenMoneyList?type=1").then(res => {
					uni.hideLoading();
					if (res.code === 0) {
						this.brandMoney = res.data.records
						let list = res.data.records
						let arr = []
						list.forEach((d, index) => {
							if (d.maxMoney > 0) {
								arr.push(d.maxMoney)
							}
							if (d.minMoney > 0) {
								arr.push(d.minMoney)
							}
						})
						console.log(arr)
						var temp = 0
						// var arr = res.data.records;
						for (let i = 0; i <= arr.length - 1; i++) {
							for (let j = 0; j <= arr.length - i; j++) {
								//前一项和后一项比较
								if (arr[j] && arr[j - 1]) {
									if (Number(arr[j]) < Number(arr[j - 1])) {
										//两值交换
										temp = arr[j];
										arr[j] = arr[j - 1];
										arr[j - 1] = temp
									}
								}
							}
						}
						// console.log(arr,arr[0].maxMoney); //[30,20,13,10,8]
						// console.log(temp, '======********')

						this.rangeMin = Number(arr[0])
						this.rangMax = Number(arr[arr.length - 1])

					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
					}
				});
			},
			// 获取标签
			getlabel() {
				this.$Request.get("/app/dict/getDictList?name=搜索标签").then(res => {
					uni.hideLoading();
					if (res.code === 0) {
						res.data.map(item => {
							item.isTrue = false
						})
						this.labelist = res.data
					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
					}
				});
			},
			chongzhi() {
				this.minCarYear = ''
				this.maxCarYear = ''
				this.minKilometers = ''
				this.maxKilometers = '';
				this.page = 1
				this.getFabuList();
				this.$refs.uDropdown.close();
			},
			bindtab() {
				this.minCarYear = this.rangeCarage[0]
				this.maxCarYear = this.rangeCarage[1]
				this.minKilometers = this.rangeValuelc[0]
				this.maxKilometers = this.rangeValuelc[1]
				console.log(this.rangeValuelc)
				this.page = 1
				this.getFabuList();
				this.$refs.uDropdown.close();
			},
			handleRangeChangecarAge(e) {
				// console.log(e)
				this.rangeCarage = e
			},
			// 获取车龄
			getCarage() {
				this.$Request.get("/app/screenMoney/getScreenMoneyList?type=2").then(res => {
					uni.hideLoading();
					if (res.code === 0) {
						this.carAge = res.data.records

						// 定义一组数字
						// var arr = res.data.records
						let list = res.data.records
						let arr = []
						list.forEach((d, index) => {
							if (d.maxMoney > 0) {
								arr.push(d.maxMoney)
							}
							if (d.minMoney > 0) {
								arr.push(d.minMoney)
							}
						})
						console.log(arr)
						let max = arr[0]
						arr.forEach(item => max = Number(item) > Number(max) ? Number(item) :
							Number(max))

						let min = arr[0]
						arr.forEach(item => min = Number(item) < Number(min) ? Number(item) :
							Number(min))

						this.carAgeMin = min
						this.carAgeMax = max
						this.rangeCarage = [this.carAgeMin, this.carAgeMax]
					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
					}
				});
			},
			handleRangeChangelc(e) {
				// console.log(e)
				this.rangeValuelc = e
			},
			// 获取里程
			getlc() {
				this.$Request.get("/app/screenMoney/getScreenMoneyList?type=3").then(res => {
					uni.hideLoading();
					if (res.code === 0) {
						// this.carAge = res.data.records
						// 定义一组数字
						// var arr = res.data.records
						let list = res.data.records
						let arr = []
						list.forEach((d, index) => {
							if (d.maxMoney) {
								arr.push(d.maxMoney)
							}
							if (d.minMoney) {
								arr.push(d.minMoney)
							}
						})
						// 获取最大值
						let max = arr[0]
						arr.forEach(item => max = Number(item) > Number(max) ? Number(item) :
							Number(max))
						//获取最小值
						let min = arr[0]
						arr.forEach(item => min = Number(item) < Number(min) ? Number(item) :
							Number(min))
						console.log('里程最大值：' + max, '最小值：' + min) // 100
						this.lcMin = min
						this.lcMax = max
						this.rangeValuelc = [this.lcMin, this.lcMax]
					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
					}
				});
			},
			//获取分类列表
			getClassListof() {
				getClassLists().then(res => {
					if (res && res.code == 0) {
						// this.gridData = res.data
						this.list = res.data.records
						this.list = JSON.parse(JSON.stringify(this.list).replace(/className/g, 'name'))
						this.classId = this.list[0].classId
						this.getFabuList();

					}
				})
			},
			// 低价  每日切换
			changee(index) {
				this.itemsIndex = index
				this.getdaylist()
			},
			scrolltolowerday() {
				if (this.count > this.pageday) {
					this.pageday += 1
					this.getdaylist()
				}

			},
			//每日上新
			getdaylist() {
				if (this.itemsIndex == 0) {
					let data = {
						page: this.pageday,
						limit: this.limit,
						city: this.city
					}
					this.$Request.getT('/app/car/getCheapCarList', data).then(res => {
						if (res.code == 0) {
							this.count = res.data.pages
							let list = res.data.records
							for (var i = 0; i < list.length; i++) {

								if (list[i].titleImg) {
									let titleImg = list[i].titleImg.split(',')
									list[i].titleImg = titleImg[0]
								}
							}

							this.daylist = list
						}
					});
				} else {
					let data = {
						page: this.pageday,
						limit: this.limit,
						city: this.city
					}
					this.$Request.getT('/app/car/getNowCarList', data).then(res => {
						if (res.code == 0) {
							let list = res.data.records
							for (var i = 0; i < list.length; i++) {

								if (list[i].titleImg) {
									let titleImg = list[i].titleImg.split(',')
									list[i].titleImg = titleImg[0]
								}
							}

							this.daylist = list
						}
					});
				}

			},
			//获取发布列表
			getFabuList() {
				// let minMoneys = ''
				// let maxMoneys = ''
				// if (this.brandMoney.length != 0) {
				// 	let minMoney = Number(this.brandMoney[0].minMoney)
				// 	let maxMoney = Number(this.brandMoney[this.brandMoney.length - 1].maxMoney)
				// 	let rangeValue = [Number(minMoney), Number(maxMoney)]
				// 	if (rangeValue[0] == this.rangeValue[0] && rangeValue[1] == this.rangeValue[1]) {
				// 		minMoneys = ''
				// 		maxMoneys = ''
				// 	} else {
				// 		minMoneys = this.rangeValue[0]
				// 		maxMoneys = this.rangeValue[1]
				// 	}
				// }

				let data = {
					page: this.page,
					limit: this.limit,
					minMoney: this.minMoney,
					maxMoney: this.maxMoney,
					brandName: this.brandName,
					isRecommend: '',
					city: this.city,
					minCarYear: this.minCarYear,
					maxCarYear: this.maxCarYear,
					label: this.label,
					moneySort: this.moneySort,
					kilometersSort: this.kilometersSort,
					carYearSort: this.carYearSort,
					minKilometers: this.minKilometers,
					maxKilometers: this.maxKilometers,
					carTypeIds: this.carlist
				}
				getDataList(data).then(res => {
					uni.hideLoading();
					uni.stopPullDownRefresh();
					if (res && res.code == 0) {
						this.count = res.data.pages
						if (this.page == this.count) {
							this.status = 'nomore'
						} else {
							this.status = 'loadmore'
						}
						let list = res.data.records
						for (var i = 0; i < list.length; i++) {
							list[i].carYear = list[i].registerTime.substring(0, 4)

							if (list[i].titleImg) {
								let titleImg = list[i].titleImg.split(',')
								list[i].titleImg = titleImg[0]
							}
						}

						if (this.page == 1) {
							this.orderlist = list
						} else {
							this.orderlist = [...this.orderlist, ...list]
						}
					}
				})
			},
			// tab切换
			change(index) {
				// #ifdef MP-WEIXIN
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				// #endif
				this.current = index;
				this.list.map((item, index) => {
					if (index === this.current) {
						this.classId = item.classId
					}
				})
				uni.showLoading({
					title: '加载中'
				})
				this.orderlist = [];
				this.page = 1
				this.getFabuList();
			},
			goCarlist(item) {
				if (uni.getStorageSync('token')) {
					// #ifdef MP-WEIXIN
					if (uni.getStorageSync('sendMsg')) {
						console.log('授权+1')
						wx.requestSubscribeMessage({
							tmplIds: this.arr,
							success(re) {
								console.log(JSON.stringify(re), 111111111111)
								var datas = JSON.stringify(re);
								if (datas.indexOf("accept") != -1) {
									console.log(re)
								}
							},
							fail: (res) => {
								console.log(res)
							}
						})
					}
					// #endif
					uni.navigateTo({
						url: '/my/buy/ershouche?brandName=' + item.name + '&isFirst=1'
					})
					// uni.navigateTo({
					// 	url: '/my/buy/ershouche?brandName=' + item.name + '&maxMoney=' + this.maxMoneyban +
					// 		'&minMoney=' + this.minMoneyban + '&labes=' + this.labesban
					// })
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					})
				}

			},
			clickItem(item) {
				if (uni.getStorageSync('token')) {
					// #ifdef MP-WEIXIN
					if (uni.getStorageSync('sendMsg')) {
						console.log('授权+1')
						wx.requestSubscribeMessage({
							tmplIds: this.arr,
							success(re) {
								console.log(JSON.stringify(re), 111111111111)
								var datas = JSON.stringify(re);
								if (datas.indexOf("accept") != -1) {
									console.log(re)
								}
							},
							fail: (res) => {
								console.log(res)
							}
						})
					}
					// #endif
					uni.navigateTo({
						url: '/pages/index/classify/detail?carId=' + item.carId
					})
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					})
				}

			},
			// 开启订阅消息
			openMsg() {
				console.log('订阅消息')
				var that = this
				uni.getSetting({
					withSubscriptions: true, //是否获取用户订阅消息的订阅状态，默认false不返回
					success(ret) {
						console.log(ret.subscriptionsSetting, '------------------')
						// if (ret.subscriptionsSetting.itemSettings && Object.keys(ret.subscriptionsSetting.itemSettings).length == 2) {
						if (ret.subscriptionsSetting.itemSettings) {
							uni.setStorageSync('sendMsg', true)
							uni.openSetting({ // 打开设置页 
								success(rea) {
									console.log(rea.authSetting)
								}
							});
						} else { // 用户没有点击“总是保持以上，不再询问”则每次都会调起订阅消息
							console.log(99999)
							uni.setStorageSync('sendMsg', false)
							uni.showModal({
								title: '提示',
								content: '为了更好的体验,请绑定消息推送',
								confirmText: '确定',
								cancelText: '取消',
								success: function(res) {
									if (res.confirm) {
										wx.requestSubscribeMessage({
											tmplIds: that.arr,
											success(re) {
												console.log(JSON.stringify(re),
													'++++++++++++++')
												var datas = JSON.stringify(re);
												if (datas.indexOf("accept") != -1) {
													console.log(re)
													// uni.setStorageSync('sendMsg', true)
												}
											},
											fail: (res) => {
												console.log(res)
											}
										})
										// uni.setStorageSync('sendMsg', true)
										console.log('确认')
										that.showModal = false
									} else if (res.cancel) {
										console.log('取消')
										// uni.setStorageSync('sendMsg', false)
										that.showModal = true
									}
								}
							})
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background: #F2F2F7;
	}

	.searchbox {
		flex: 1;
		// width: 90%;
		height: 70rpx;
		background: #FFFFFF;
		border: 1px solid #60B7FF;
		border-radius: 35rpx;
		display: flex;
		align-items: center;
		justify-content: start;
		padding: 0 0 0 30rpx;
	}

	.headsr {
		background: linear-gradient(to bottom, #EAF9FF 20%, #F2F6FC 80%);
		// background-image: url('../../static/index/bj1.png');
		// background-size: 100% 100%;
		width: 100%;
		// height: 148rpx;
		position: fixed;
		top: 0rpx;
		left: 0;
		right: 0;
		z-index: 999;
		/* #ifdef APP */
		padding-top: 120rpx;
		/* #endif */
		/* #ifdef H5 */
		padding-top: 30rpx;
		/* #endif */
		/* #ifdef MP-WEIXIN */
		padding-top: 0rpx;
		padding-bottom: 0;
		/* #endif */
	}

	.headapp {
		/* #ifdef APP */
		padding-top: 230rpx;
		/* #endif */
		/* #ifdef H5 */
		padding-top: 160rpx;
		/* #endif */
		/* #ifdef MP-WEIXIN */
		padding-top: 150rpx;
		/* #endif */
	}

	.u-close {
		position: absolute;
		top: 32rpx;
		right: 32rpx;
	}

	
	.message {
		width: 90%;
		white-space: nowrap;
	}
	
	.scroll-view-item_H {
		display: inline-block;
	}
	
	.scroll-view-item_H image {
		width: 110rpx;
		height: 27rpx;
	}
	
	.index_gonggao {
		text-align: left;
		width: 500rpx;
		height: 70rpx;
		line-height: 100rpx;
		display: flex;
		align-items: center;
	}
	
	.images {
		width: 100rpx;
		height: 100rpx;
	}
	


	.flowListStyle {
		width: 100%;
		height: auto;
		display: flex;
		justify-content: center;
		margin-top: 20rpx;

		.flowListStyle-box {
			width: 686rpx;
			height: 100%;



		}
	}

	.btn {
		padding: 5rpx 10rpx;
		color: #666666;
		background: #f2f2f2;
		border-radius: 8rpx;
		margin-right: 8rpx;
	}

	.announcement {
		padding: 20rpx;
		width: 93%;
		height: 340rpx;
		background: #FFFFFF;
		border-radius: 12rpx;
		margin: 0 auto;
	}

	.announcementbox {
		width: 100%;
		display: flex;
		justify-content: space-around;
		align-items: center;
		padding: 15rpx 0;

		.scroll-view_H {
			white-space: nowrap;
			width: 100%;

		}

		.scroll-view-item_H {
			display: inline-block;
			width: 20%;
			text-align: center;

			.tt {
				margin: 0 auto;
				font-size: 26rpx;
				color: #333333;
				width: 100rpx;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;


			}

		}

		.scroll-view-item_Price {
			display: inline-block;
			// width: 20%;
			padding: 0 20rpx;
			// width: 121rpx;
			height: 60rpx;
			background: #F5F5FA;
			border-radius: 8rpx;
			text-align: center;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
			// line-height: 60rpx;
			margin-right: 12rpx;
			display: inline-flex;
			align-items: center;
			justify-content: center;

		}

		.scroll-view-item_PriceAct {
			background: #EAF9FF;
			color: #2979ff;
		}

		.scroll-view-item_labe {
			display: inline-block;
			// width: 20%;
			padding: 0 35rpx;
			// width: 121rpx;
			height: 60rpx;
			background: #F5F5FA;
			border-radius: 8rpx;
			text-align: center;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
			// line-height: 60rpx;
			margin-right: 12rpx;
			display: inline-flex;
			align-items: center;
			justify-content: center;
		}

		.scroll-view-item_labeAct {
			background: #EAF9FF;
			color: #2979ff;
		}


	}

	.category {
		width: 100%;

		.dots {
			display: flex;
			justify-content: center;
			height: 15upx;
			width: 100%;

			view {
				width: 30upx;
				height: 5upx;
				background-color: rgba(0, 0, 0, 0.2);

				&.active {
					background-color: #ff570a;
				}
			}
		}

		.swiper {
			width: 100%;
			padding: 10upx 0;

			.uni-swiper-dot {
				width: 20upx;
			}

			.category-list {
				width: 100%;
				height: auto;
				display: flex;
				justify-content: flex-start;
				padding: 10upx 0;
				flex-flow: wrap;

				.icon {
					width: 25%;
					font-size: 26upx;
					color: #666;
					text-align: center;
				}
			}
		}
	}

	.tabberN {
		padding: 0rpx 30rpx 10rpx 30rpx;

		.end-title {
			display: flex;
			width: 686rpx;
			height: 100rpx;
			line-height: 100rpx;
			background: #EFF3FC;
			border-radius: 12rpx;
			font-size: 28rpx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #8993A3;
		}

		.end-title view {
			width: 35%;
			text-align: center;
		}

		.end-cont {
			padding: 20rpx;
			width: 100%;
			height: 290rpx;
			background: #fff;
			border-radius: 0 12rpx 12rpx 12rpx;

			.scroll-view_H {
				white-space: nowrap;
				width: 100%;
			}

			.scroll-view-item_H {
				display: inline-block;
				width: 202rpx;
				margin-right: 20rpx;

				.names {
					width: 202rpx;
					font-size: 28rpx;
					font-family: PingFang SC;
					color: #333333;
				}
			}
		}

		.btna {
			background: #FFFFFF;
			border-radius: 12rpx 12rpx 0 0;
			font-size: 32rpx;
			font-family: PingFang SC;
			font-weight: 800;
			color: #333333;
			position: relative;

			.imgeswx {
				position: absolute;
				bottom: 15rpx;
				left: 46%;
				z-index: 9;
			}
		}

		.dis {
			display: block;
		}
	}

	.kuai {
		width: 150rpx;
		height: 60rpx;
		background: #FFFFFF;
		border-radius: 8rpx;
		line-height: 60rpx;
		text-align: center;
	}

	.labelsx {
		padding: 0 30rpx;

		.scroll-view_H {
			white-space: nowrap;
			width: 100%;
		}

		.scroll-view-item_labe {
			display: inline-block;
			// width: 20%;
			padding: 0 35rpx;
			// width: 121rpx;
			height: 60rpx;
			background: #FFFFFF;
			border-radius: 8rpx;
			text-align: center;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
			// line-height: 60rpx;
			margin-right: 20rpx;
			display: inline-flex;
			align-items: center;
			justify-content: center;
		}

		.scroll-view-item_labeAct {
			background: #EAF9FF;
			color: #2979ff;
		}
	}

	// 价格 车龄筛选  
	.slot-content {
		background: #FFFFFF;

		.listpp {
			// text-align: center;
			padding: 0rpx 20rpx;
			// width: 150rpx;
			height: 60rpx;
			background: #F7F8FC;
			border-radius: 8rpx;
			display: flex;
			align-items: center;
			justify-content: center;
			margin-right: 20rpx;
			margin-bottom: 20rpx;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #171717;
		}

		.listppAct {
			padding: 0rpx 20rpx;
			height: 60rpx;
			display: flex;
			align-items: center;
			justify-content: center;
			background: #D4EBFF;
			border: 1px solid #2096FF;
			border-radius: 8rpx;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #2096FF;
		}

		.btn {
			background: #2096FF;
			color: #FFF;
			padding: 20rpx 0;
			text-align: center;
		}
	}

	// /deep/.u-dropdown__menu {
	// 	width: 90%;
	// 	left: 65px !important;
	// }

	.tuij {
		position: absolute;
		top: 20rpx;
		left: 30rpx;
		z-index: 99;
	}

	.acttj {
		color: #2979ff;
		position: absolute;
		top: 20rpx;
		left: 30rpx;
		z-index: 99;
	}
</style>