<template>
	<view>
		<ul >
			<li v-for="(item,index) in dataArr">
				<image :src="item" mode="aspectFill"></image>
				<view class="iconfont icon-ziyuan delete-photo" @tap="deletePhoto(index)"></view>
			</li>
			<li>
				<view class="iconfont icon-jiahao add-photo" @tap="addPhoto()"></view>
			</li>
		</ul>
	</view>
</template>

<script>
	export default {
		name:"photoPicker",
		data() {
			return {
				dataArr:[],
				
			};
		},
		methods:{
			deletePhoto(index){
				let arr = this.dataArr;
				for(let i = index; i < arr.length-1; i++){
					arr[i] = arr[i+1]
				}
				arr.pop();
				this.dataArr = arr;
			},
			addPhoto(){
				let _this = this;
				uni.chooseImage({
					count: 1,//允许上传的照片数量
					sizeType: ['original', 'compressed'],
					success(res) {
						console.log(res)
						let url = res.tempFilePaths[0];
						_this.dataArr.push(url); 
					}
				})
				
			},
			
		}
	}
</script>

<style>
	ul{
		width: 600rpx;
		display: flex;
		flex-wrap: wrap;
		/* justify-content: space-between; */
		/* margin: 30rpx 100rpx; */
		margin: 30rpx auto;
	}
	li{
		width: 200rpx;
		height: 150rpx;
		list-style: none;
		text-align: center;
	}
	
	.delete-photo{
		color: #FF0000;
		font-size: 35rpx;
		width: 35rpx;
		height: 35rpx;
		margin-top: -110rpx;
		margin-left: 80rpx;
		background-color: #fff;
		border-radius: 50%;
	}
	
	li image{
		width: 100rpx;
		height: 100rpx;
		display: table;
		text-align:center;
		
	}
	li .add-photo{
		width: 100rpx;
		height: 100rpx;
		font-size: 50rpx;
		padding: 25rpx;
		border: 1rpx #999999 dotted;
		vertical-align: middle
	}
	

</style>
