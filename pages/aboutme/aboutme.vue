<template>
	<view class="aboutme">
		<view class="my-header">
			<view class="my-image">
				<image :src="userInfo.avatarUrl" mode=""></image>
			</view>
			<view>
				<view class="my-name">
					{{userInfo.nickName}}
				</view>
				<view class="my-address">
					{{getUserAddress()}}
				</view>
			</view>
		</view>
		<view class="my-content">
			<!-- 修改基本信息 -->
			<view class="my-content-item" @tap="toModifyPhone">绑定手机号</view>
			
		</view>
		<view class="my-footer">
			<button type="warn" @tap="logout()">退出登录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo:null,
				user:null
			}
		},
		methods: {
			getUserAddress(){
				return this.userInfo.country+'，'+this.userInfo.province+this.userInfo.city;
			},
			toModifyPhone(){
				uni.navigateTo({
					url:"../bindPhone/bindPhone"
				})
			},
			logout(){
				//请空缓存
				uni.clearStorage()
				uni.reLaunch({
					url:"../login/login"
				})
			}
		},
		created() {
			this.userInfo = JSON.parse( uni.getStorageSync("userInfo"));
			this.user = JSON.parse( uni.getStorageSync("user"));
			console.log(this.userInfo)
		}
	}
</script>

<style>
	.aboutme{
		/* position: relative; */
		
	}
	.my-header{
		display: flex;
		align-items: center;
		padding: 30rpx 30rpx;
	}
	.my-image{
		padding: 30rpx;
	}
	.my-image image{
		width: 180rpx;
		height: 180rpx;
		border-radius: 50%;
	}
	.my-name{
		font-size: 40rpx;
	}
	.my-content-item{
		font-size: 35rpx;
		color: #585858;
		padding: 30rpx 70rpx;
		border-top: #eaeaea 1rpx solid;
		border-bottom: #eaeaea 1rpx solid;
	}
	.my-footer button{
		position: absolute;
		width: 500rpx;
		left: 50%;
		top: 80%;
		transform: translate(-50%,-50%);
		
	}
</style>
