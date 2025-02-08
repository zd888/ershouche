<template>
	<view style="width: 100%;">
		<web-view :src="url" style="width: 100%;"></web-view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				url: null, //要打开的外部链接
				viewerUrl: '/hybrid/html/web/viewer.html',
				webviewStyles: {
					width: '750px',
					height: '100%',
				},

			}
		},
		onReady() {
			uni.setNavigationBarTitle({
				title:'预览'
			})
			// #ifdef APP-PLUS
			console.log('App plus2')
			var currentWebview = this.$mp.page.$getAppWebview() //获取当前页面的webview对象
			setTimeout(function() {
				wv = currentWebview.children()[0]
				wv.setStyle({
					scalable: true
				})
			}, 1000); //如果是页面初始化调用时，需要延时一下
			// #endif
		},
		onLoad: function(option) {
			this.url = option.url

		}
	}
</script>
<style lang="scss">
	.webview {
		width: 100%;
		height: 100%;
	}
</style>