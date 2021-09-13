<template>
	<view class="loginmain">
		<view class="loginimg">

			<image class="loginimg" src="../../static/logo.png" mode=""></image>
		</view>
		<view @click="login" class="login">
			微信一建登录
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				openid: '',
				nickName: '',
				icon: '',
				info: '',
				thon: ''
			}
		},
		beforeCreate(){
			let token = uni.getStorageSync("token")
			if (token) {
				console.log(token)
				uni.switchTab({
					url: '/pages/index/index'
				});
			}
		},
		methods: {
			login() {
				let than = this
				uni.login({
					success: (res) => {
						console.log(res)
						uni.request({
							url: 'http://api_devs.wanxikeji.cn/api/codeExchangeOpenid',
							data: {
								code: res.code
							},
							success(ress) {
								console.log(ress, ress.data.data.openid)
								than.openid = ress.data.data.openid
								than.info = ress.data.data.info
								if (ress.data.data.info) {
									console.log(11, ress.data.data.info.token)
									try {
										uni.setStorageSync('token', ress.data.data.info.token);
									} catch (e) {
										console.log(e)
									}
								}

							}
						})
					}
				})
				uni.getUserProfile({
					desc: '展示用户信息',
					success: (res) => {
						res.rawData = JSON.parse(res.rawData);
						console.log(res.rawData)
						// than.openid
						// nickName icon
						than.nickName=res.rawData.nickName
						than.icon=res.rawData.avatarUrl
						let token = uni.getStorageSync("token")
						if (token) {
							console.log(token)
							uni.switchTab({
								url: '/pages/index/index'
							});
						}
						else {
							
							uni.request({
								url: 'http://api_devs.wanxikeji.cn/api/register',
								method: "post",
								data: {
									openid: than.openid,
									nick_name:than.nickName,
									icon:than.icon
								},
								success(val) {
									uni.switchTab({
										url: '/pages/index/index'
									});
									try {
										uni.setStorageSync('token', val.data.data
											.token);
									} catch (e) {
										console.log(e)
										// error
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

<style>
	.loginimg {
		width: 600rpx;
		margin: 0 auto;
	}

	.login {
		width: 400rpx;
		padding: 20rpx;
		text-align: center;
		border-radius: 44rpx;
		margin: 50rpx auto;
		background-color: #F2310C;
	}
</style>
