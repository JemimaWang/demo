<template>
	<view class="login">
		<view class="canIUse||canIGetUserProfile">
			<view class='login-header'>
				<image style="width: 140rpx; height: 140rpx;" mode="aspectFit" src="../../static/hospital.png"></image>
			</view>
		</view>
		<view class="login-box">
			 <!--新版登录方式-->
			<button v-if="canIGetUserProfile" class='login-btn' type='primary' @click="bindGetUserInfo"> 授权登录 </button>
			<!--旧版登录方式-->
			<button v-else class='login-btn' type='primary' open-type="getUserInfo" withCredentials="true" lang="zh_CN" @getuserinfo="bindGetUserInfo"> 授权登录 </button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				canIGetUserProfile:false,
				userInfo:{},
				canIUse: uni.canIUse('button.open-type.getUserInfo'),

			}
		},
		created() {
			let userStr = '';
			userStr = uni.getStorageSync('user');
			if (userStr != '') {
				console.log(userStr)
				uni.switchTab({
					url: "../index/index"
				})
			}
		},
		methods: {
			//登录授权
			bindGetUserInfo(e) {
				var _this = this;
				if(this.canIGetUserProfile){
					uni.getUserProfile({
						lang: 'zh_CN',
						desc: '登录',
						success: (res) => {
							_this.userInfo = res.userInfo;
							console.log(res.userInfo);
							_this.login();
							
						}
					})
				}else{
                    //旧版登录方式
					if (e.detail.userInfo) {
						//用户按了允许授权按钮
						//console.log('手动');
						//console.log(e.detail.userInfo);
						_this.userInfo = e.detail.userInfo;
						try {
							_this.login();
						} catch (e) {
						}
					} else {
						console.log('用户拒绝了授权');
					    //用户按了拒绝按钮
					}
				}
				
			},
			login() {
				var _this = this;
				uni.login({
					provider: 'weixin',
					success: res=> {
						if(!res.code){
							uni.showToast({title: '登录失败！',duration: 2000});
							return;
						}
						uni.request({
							url: "http://localhost:9000/getOpenId",
							data:{
								appid: 'wx34e1c4b6aa49becd',
								code: res.code,
								secret: 'b483bcd460085e79df0a6f6d0201d669'
							},
							success: result => {
								console.log(result)
								let openid = result.data.openid;
								_this.toIndex(openid);
								
							}
						})
					}
				})
			},
			async toIndex(openId) {
				var [error,res] = await uni.request({
					url: "http://47.102.159.24:9000/selectPersonAccout",
					data: {
						openId: openId
					},
				})
				let data = null;
				//如果找不到用户
				if (res.data.data === null || res.data.data === undefined) {
					var [error2,res2] = await uni.request({
						url: "http://47.102.159.24:9000/addAccout",
						method: "POST",
						data: {
							userType: 1,
							miniOpenId: openId,
							phoneNo: ''
						},
						
					});
					data = res2.data.data;
				}else{
					data = res.data.data;
				}
						
				console.log(data)
				let user = {
					type: data.userType,
					openId: data.miniOpenId,
					phone: data.phoneNo,
					userId:data.userId,
					createTime:data.createTime
				}
				console.log(user)
				uni.setStorageSync('user', JSON.stringify(user));
				uni.setStorageSync('userInfo', JSON.stringify(this.userInfo))
				uni.switchTab({
					url: "../index/index"
				})
			}
		},
		onLoad() {
			var _this = this;
			//console.log(uni.getUserProfile);
			if( uni.getUserProfile ){  
				this.canIGetUserProfile = true;  
			} 
			
			//判断若是版本不支持新版则采用旧版登录方式
			//查看是否授权
			if( !this.canIGetUserProfile){  
				uni.getSetting({
					success: function(res) {
						if (res.authSetting['scope.userInfo']) {
							uni.getUserInfo({
							  provider: 'weixin',
							  success: function(res) {
								//console.log(res);
								_this.userInfo = res.userInfo;
								try {
								  _this.login();
								} catch (e) {}
							  },
							  fail(res) {}
							});
						} else {
							// 用户没有授权
							console.log('用户还没有授权');
						}
					}
				});
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

	.login-box text {
		margin: 10rpx;
	}
</style>
