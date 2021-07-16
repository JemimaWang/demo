<template>
	<view class="login">
		<view class="canIUse||canIGetUserProfile">
			<view class='login-header'>
				<image style="width: 140rpx; height: 140rpx;" mode="aspectFit" src="../../static/hospital.png"></image>
			</view>
		</view>
		<view class="login-box">
			<button class='login-btn iconfont icon-weixin' type='primary' @click="bindGetUserInfo"><text>微信快捷登录</text></button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		created() {
			let userStr = '';
			userStr = uni.getStorageSync('user');
			if(userStr!=''){
				console.log(userStr)
				uni.redirectTo({
					url:"../index/index"
				})
			}
		},
		methods: {
			bindGetUserInfo(){
				var _this = this;
				uni.getUserProfile({
					desc:'登录',
					success:(res)=>{
						_this.userInfo = res.userInfo;
						console.log(res.userInfo);
						_this.login();
					}
				})
			},
			login:function(){
				var _this = this;
				uni.login({
					provider:'weixin',
					success: function(res){
						//获取临时登录凭证code
						let code = res.code;
						//获取openid，session_key
						let appid = 'wx978c5afc9cab45f1';
						let secret = '188613002bf7a670baebaddcc73a62c4';
						let url = 'https://api.weixin.qq.com/sns/jscode2session?appid=' + appid + '&secret=' + secret + '&js_code=' +
							code + '&grant_type=authorization_code';
						uni.request({
							url: url,
							success: result => {
								console.info(result.data.openid);
								let openid = result.data.openid;
								/* 待更改：
								 *  根据openid在base_account表中寻找用户或医生，
								 * 		若找到，则直接登录
								 * 		若未找到，将下面的userInfo添加到表中，再登录
								 * 
								*/
								let user={
									type: '1',
									openId: result.data.openid,
									phone: '无',
									createTime: new Date()
								}
								console.log(user)
								uni.setStorageSync('user',JSON.stringify(user));
								uni.setStorageSync('userInfo',JSON.stringify(_this.userInfo))
								uni.redirectTo({
									url:"../index/index"
								})
							}
						})
					}
				})
			}
		}
	}
</script>

<style>
	.login-header {
		margin: 90rpx 0 90rpx 50rpx;
		border-bottom: 1px solid #ccc;
		text-align: center;
		width: 650rpx;
		height: 300rpx;
		line-height: 450rpx;
	}

	.login-header image {
		width: 200rpx;
		height: 200rpx;
	}

	/* .login-content {
		margin-left: 50rpx;
		margin-bottom: 90rpx;
	} */

	/* .login-content text {
		display: block;
		color: #9d9d9d;
		margin-top: 40rpx;
	} */

	.login-btn {
		border-radius: 80rpx;
		margin: 70rpx 50rpx;
		font-size: 35rpx;
		white-space: pre-wrap;
	}
	.login-box text{
		margin: 10rpx;
	}

</style>