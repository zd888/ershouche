<template>
	<view class="container">
		<view class="wrapper">
			<view class="input-content">
				<view class="cu-form-group" style="border: 2upx solid whitesmoke;margin-bottom: 20px;border-radius: 30px">
					<view class="title text-black">手机号</view>
					<input type="number" :value="phone" placeholder="请输入手机号" maxlength="11" data-key="phone" @input="inputChange" />
				</view>
				<view class="cu-form-group" style="border: 2upx solid whitesmoke;margin-bottom: 20px;border-radius: 30px">
					<text class="title text-black">验证码</text>
					<input type="number" :value="code" placeholder="请输入验证码" maxlength="6" data-key="code" @input="inputChange"
					 @confirm="toLogin" />
					<button class="send-msg" @click="sendMsgs" :disabled="sending">{{sendTime}}</button>
				</view>
				<view class="cu-form-group" style="border: 2upx solid whitesmoke;margin-bottom: 20px;border-radius: 30px">
					<text class="title text-black">设置密码</text>
					<input type="password" :value="password" placeholder="请设置新密码" placeholder-class="input-empty" maxlength="20"
					 minlength="6" data-key="password" @input="inputChange" @confirm="toLogin" />
				</view>
			</view>
			<button class="confirm-btn" @click="toLogin">立即找回</button>
		</view>
	</view>
</template>

<script>
	import {sendMsg,forgetPwd} from '@/apis/login.js'
	export default {
		data() {
			return {
				code: '',
				phone: '',
				password: '',
				sending: false,
				sendTime: '获取验证码',
				count: 60,
				logining: false
			}
		},

		methods: {
			sendMsgs() {
				const {
					phone
				} = this;
				if (!phone) {
					this.$queue.showToast("请输入手机号");
				} else if (phone.length !== 11) {
					this.$queue.showToast("请输入正确的手机号");
				} else {
					this.$queue.showLoading("正在发送验证码...");
					let data = {
						phone: phone
					}
					sendMsg(data).then(res => {
						console.log(res)
						if (res.code === 0) {
							this.sending = true;
							this.$queue.showToast('验证码发送成功请注意查收');
							this.countDown();
							uni.hideLoading();
						} else {
							uni.hideLoading();
							uni.showModal({
								showCancel: false,
								title: '短信发送失败',
								content: res.msg ? res.msg : '请一分钟后再获取验证码'
							});
						}
					})
				}
			},
			countDown() {
				const {
					count
				} = this;
				if (count === 1) {
					this.count = 60;
					this.sending = false;
					this.sendTime = '获取验证码'
				} else {
					this.count = count - 1;
					this.sending = true;
					this.sendTime = count - 1 + '秒后重新获取';
					setTimeout(this.countDown.bind(this), 1000);
				}
			},
			inputChange(e) {
				const key = e.currentTarget.dataset.key;
				this[key] = e.detail.value;
			},
			navBack() {
				uni.navigateBack();
			},


			navTo(url) {
				uni.navigateTo({
					url
				})
			},
			toLogin() {
				const {
					phone,
					password,
					code
				} = this;
				if (!phone) {
					this.$queue.showToast("请输入手机号");
				} else if (!password) {
					this.$queue.showToast("请设置密码");
				} else if (password.length < 6) {
					this.$queue.showToast("密码位数必须大于六位");
				} else {
					this.logining = true;
					this.$queue.showLoading("正在修改密码中...");
					let data = {
						pwd: password,
						phone: phone,
						msg: code
					}
					forgetPwd(data).then(res=>{
						if(res.code == 0){
							uni.navigateBack()
						}else{
							uni.showModal({
								showCancel: false,
								title: '密码找回失败',
								content: res.msg,
							});
						}
					})
					// this.$Request.post("/app/Login/forgetPwd", {
					// 	pwd: password,
					// 	phone: phone,
					// 	msg: code
					// }).then(res => {
					// 	uni.hideLoading();
					// 	if (res.code === 0) {
					// 		uni.navigateTo({
					// 			url: '/pages/public/loginphone'
					// 		});
					// 	} else {

					// 		uni.showModal({
					// 			showCancel: false,
					// 			title: '密码找回失败',
					// 			content: res.msg,
					// 		});
					// 	}
					// });
				}
			},
		},

	}
</script>

<style lang='scss'>
	page {
		height: 100%;
		background: #fff;
	}

	.send-msg {
		border-radius: 30px;
		color: white;
		height: 30px;
		font-size: 14px;
		line-height: 30px;
		background: #4498FF;
	}

	.container {
		padding-top: 32upx;
		position: relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}

	.wrapper {
		position: relative;
		z-index: 90;
		padding-bottom: 20px;
	}


	.input-content {
		padding: 32upx 80upx;
	}


	.confirm-btn {
		width: 600upx;
		height: 80upx;
		line-height: 80upx;
		border-radius: 60upx;
		margin-top: 32upx;
		background: #4498FF;
		color: #fff;
		font-size: 32upx;

		&:after {
			border-radius: 60px;
		}
	}

</style>
