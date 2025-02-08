<template>
	<view>
		<view class="flex flex-wrap justify-between">
			<view class="demo-warter" v-for="(item, index) in orderlist" :key="index"
				@click="clickItem(item)">
				<image class="demo-warter-img" :src="item.titleImg?item.titleImg:'../../static/logo.png'"
					mode="aspectFill">
				</image>
				<view class="demo-title">
					<view class="demo-title-box">
						{{item.brandName}} {{item.modelsName}} {{item.carName}}
					</view>
				</view>
				<view class="demo-distances">
					{{item.carYear}}年/{{item.kilometers}}万公里
				</view>
				<view class="demo-lable" v-if="item.label">
					<view class="demo-lable-item">{{item.label}}</view>
				</view>
				<view class="demo-distance">
					<text>{{item.money}}万</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			orderlist: {
				type: Array,
				default: () => []
			},

		},
		data() {
			return{}
		},
		mounted() {
			
		},
		methods: {
			clickItem(e) {
				this.$emit('clickItem', e);
			}
		}
	}
</script>

<style lang="less">
	.demo-warter {
		width: 333rpx;
		height: auto;
		border-radius: 24rpx;
		background-color: #F8F9FD;
		position: relative;
		margin-bottom: 20rpx;

		.demo-warter-img {
			width: 100%;
			height: 256rpx;
			border-radius: 24rpx 24rpx 0 0;
		}

		.demo-title {
			width: 100%;
			font-size: 30rpx;
			margin-top: 15rpx;
			color: #333333;
			font-weight: 700;
			padding-left: 16rpx;
			padding-right: 16rpx;
			display: flex;
			justify-content: center;

			.demo-title-box {
				width: 290rpx;
				text-overflow: -o-ellipsis-lastline;
				overflow: hidden;
				text-overflow: ellipsis;
				display: -webkit-box;
				-webkit-line-clamp: 2; //可设置显示的行数
				line-clamp: 2;
				-webkit-box-orient: vertical;
			}
		}
		.demo-distance {
			display: flex;
			align-items: center;
			padding: 18rpx;
		
			text {
				margin-right: 16rpx;
				color: #FF6800;
				font-size: 32rpx;
				font-weight: 700;
			}
		}
		
		.demo-distances {
			font-size: 26rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #9D9EAB;
			padding: 10rpx 0 16rpx 16rpx;
		}
		
		.demo-lable {
			width: 100%;
			padding: 0rpx 16rpx 0rpx 16rpx;
			display: flex;
			align-items: center;
			flex-wrap: wrap;
		
			.demo-lable-item {
				// width: 104rpx;
				padding: 10rpx;
				height: 32rpx;
				background: #f2f2f2;
				border-radius: 4rpx;
				color: #666666;
				font-size: 22rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				margin-right: 8rpx;
				margin-bottom: 8rpx;
			}
		}
		
	}
</style>