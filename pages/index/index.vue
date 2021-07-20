<template>
	<view class="content">
		<view @click="toDispensing()" class="index-item">复诊配药</view>
		<view @click="toRecord()" class="index-item">配药记录</view>
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				title: 'Hello',
				userInfo:{},
			}
		},
		onLoad() {
			 // #ifdef MP-WEIXIN
			 if(wx.hideHomeButton){  
			     wx.hideHomeButton();  
			 }  
			 // #endif
		},
		methods: {
			toDispensing(){
				uni.navigateTo({
					url:"../dispensing/dispensing"
				})
			},
			toRecord(){
				uni.navigateTo({
					url:"../medicineRecord/medicineRecord"
				})
			}
			
		}
	}
</script>

<style>
	.index-item{
		border: 1rpx solid #28B294;
		border-radius: 50rpx;
		padding: 70rpx 0;
		margin: 40rpx;
		font-size: 50rpx;
		color: #28B294;
		text-align: center;
	}
</style>
