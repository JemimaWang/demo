<template>
	<view class="doctor">
		<view  class="doctor-img">
			<image src="https://qbkeass.cn/fzpy/image/doctors/doctor.png"></image>
		</view>
		<view>
			<view class="doctor-info">
				<view class="doctor-name">方红全</view>
				<view class="level-name">主治医生</view>
			</view>
			<view class="dept-name">呼吸内科</view>
		</view>
		<view class="select iconfont change-doctor">更换医生&#xe60d;</view>
	</view>
</template>
<script>
	export default{
		
	}
</script>
<style>
	.doctor{
		display: flex;
		align-items: center;
		border-bottom: 15rpx solid #f2f2f2;
	}
	.doctor-img{
		margin: 20rpx 40rpx;
		margin-right: 20rpx;
		margin-top: 30rpx;
	}
	.doctor-img image{
		border: 1rpx solid #8F8F94;
		width: 150rpx;
		height: 150rpx;
		border-radius: 50%;
	}
	.doctor-info{
		display: flex;
		align-items: center;
	}
	.doctor-name{
		color: #4b4b4b;
		padding: 20rpx 0;
		font-size: 35rpx;
		font-weight: bold;
	}
	.level-name{
		border: 1rpx solid #f2aa77;
		border-radius: 25rpx;
		vertical-align: center;
		font-size: 25rpx;
		line-height: 25rpx;
		height: 30rpx;
		padding: 0 5rpx;
		color: #f2aa77;
		
	}
	.dept-name{
		font-size: 30rpx;
		padding: 20rpx 0;
		padding-top: 0;								
	}
	 .change-doctor{
		 font-size: 30rpx;
		 margin-left: 130rpx;
		 color: #cfcfcf;
	 }
</style>