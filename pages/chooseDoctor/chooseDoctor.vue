<template>
	<view>
		<view class="choose-header">
			<picker mode="selector" :range="depts" @change="bindPickerChange" :value="index">
				<view class="iconfont icon-icon-test">{{depts[index]}}</view>
			</picker>
		</view>
		<ul>
			<li v-for="(item,index) in doctorGroup" class="list-item" @tap="chooseDoctor(index)">
				<view class="list-img"><image :src="item.avatarUrl"></image></view>
				<view class="list-doctor">
					<view class="list-doctor-status">
						<view class="list-doctor-name">{{item.doctorName}}</view>
						<view class="list-doctor-level">{{item.levelName}}</view>
					</view>
					<view class="list-doctor-dept">{{item.deptName}}</view>
				</view>
			</li>
		</ul>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				depts:[],
				index:0,
				doctorGroup:[],
				doctors:[]
			}
		},
		methods: {
			getDeps(){
				let doctors = this.doctors;
				let depts = [];
				for(let item of doctors){
					if(depts.indexOf(item.deptName)<0){
						depts.push(item.deptName)
					}
				}
				this.depts = depts;
			},
			bindPickerChange(e){
				this.index = e.target.value
				this.getDoctorsOfDepts();
			},
			getDoctorsOfDepts(){
				let doctors = this.doctors;
				let group = [];
				let currentDepts = this.depts[this.index]
				for(let item of doctors){
					if(currentDepts===item.deptName){
						group.push(item);
					}
				}
				this.doctorGroup = group;
			},
			chooseDoctor(index){
				//返回
				var pages = getCurrentPages();
				var prevPage = pages[pages.length - 2]; //上一个页面
				console.log(prevPage)
				let object = {
					name:'chooseDoctor',
					doctor:this.doctorGroup[index]
				}
				prevPage.onShow(object)
				uni.navigateBack();
			}
			
		},
		created() {
			let _this = this;
			uni.request({
				url:"http://47.102.159.24:9000/getDoctor",
				success:res=>{
					console.log(res)
					_this.doctors = res.data.data
					_this.getDeps();
					_this.getDoctorsOfDepts();
					console.log(_this.doctors)
				}
			})
			
			console.log(this.depts)
			console.log(this.doctorGroup)
		}
	}
</script>

<style>
	.choose-header{
		text-align: center;
		padding: 10rpx 30rpx;
	}
	picker{
		border: 1rpx solid #666666;
	}
	.list-item{
		display: flex;
		align-items: center;
		border-bottom: 1rpx solid #BFBFBF;
	}
	.list-img{
		padding: 30rpx 50rpx;
	}
	.list-img image{
		width: 100rpx;
		height: 100rpx;
		border-radius: 50%;
		border: 1rpx solid #BFBFBF;
	}
	.list-doctor-status{
		display: flex;
		align-items: center;
		margin-left: 20rpx;
	}
	.list-doctor-name{
		font-size: 30rpx;
	}
	.list-doctor-level{
		font-size: 25rpx;
		border: 1rpx solid #f2aa77;
		border-radius: 25rpx;
		vertical-align: center;
		font-size: 25rpx;
		line-height: 25rpx;
		height: 30rpx;
		padding: 0 5rpx;
		color: #f2aa77;
	}
</style>
