<!-- 配药记录 -->
<template>
	<view>
		<view class="record-item" v-for="(item,index) in records">
			<view class="record-header">
				<view class="record-time">问诊时间：{{getCreateTime(index)}}</view>
				<view class="record-status">{{getStatus(index)}}</view>
			</view>
			<view class="record-content">
				<view class="record-doctor-img"><image :src="item.doctorImg" mode=""></image></view>
				<view class="record-other">
					<view class="record-doctor">
						<view class="record-doctor-name">{{item.doctorName}}</view>
						<view>在线云诊台</view>
					</view>
					<view class="record-person-name">就诊人：{{item.personName}}</view>
					<view class="record-ill">病情：{{item.diagnosis}}</view>
				</view>
			</view>
			<view class="record-footer">
				<view class="record-footer-content">
					<text class="iconfont icon-fuzhenpeiyao" style="color: #ccc;"></text>
					复诊配药
				</view>
				<view class="record-footer-button" v-if="item.consultStatus===3" @tap="toPrescription(index)">查看处方</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				records:[]
			}
		},
		methods: {
			getCreateTime(index){
				let time = this.records[index].createTime;
				if(time===null){
					return '';
				}else{
					let date = new Date(time)
					return date.format('yyyy-MM-dd hh:mm');
				}
			},
			getStatus(index){
				let status = this.records[index].consultStatus;
				if(status===1){
					return '待接诊';
				}else if(status===2){
					return '进行中';
				}else if(status===3){
					return '已完成';
				}
			},
			toPrescription(index){
				//待修改
				//根据问诊id可以得到处方id，每个问诊id对应一个处方id
				//可以只传入consultId，在处方界面获得处方信息
				//也可以在此界面获得处方的信息，把处方的信息传给处方界面
				uni.navigateTo({
					// url:"../prescription/prescription?consultId="+this.durgs[index].consultId
					url:"../prescription/prescription?consultAsk="+encodeURIComponent(JSON.stringify(this.records[index]))
				})
			}
			
		},
		created() {
			let user = JSON.parse(uni.getStorageSync("user"));
			let doctors = null;
			let _this = this;
			console.log(user.userId)
			uni.request({
				url:"http://47.102.159.24:9000/getDoctor",
				success:res=>{
					doctors = res.data.data;
					uni.request({
						url:"http://47.102.159.24:9000/selectUserConsult",
						data:{
							createUserId:user.userId
						},
						success:res2=>{
							_this.records = res2.data.data;
							console.log(_this.records)
							console.log(doctors)
							//获取医生照片
							for(let item of _this.records){
								for(let doctor of doctors){
									if(item.doctorId===doctor.doctorId){
										item.doctorImg = doctor.avatarUrl
									}
								}
							}
							
						}
					});
				}
				
			})
			
			
			
		}
	}
</script>

<style>
	.record-item{
		border-bottom: 10rpx solid #f3f3f3;
	}
	.record-header{
		border-bottom: 3rpx solid #f3f3f3;
		justify-content: space-between;
		padding: 20rpx;
	}
	.record-footer{
		justify-content: space-between;
		padding: 20rpx;
	}
	.record-content{
		border-bottom: 3rpx solid #f3f3f3;
		padding: 20rpx 10rpx;
	}
	.record-header,.record-content,.record-footer{
		display: flex;
		align-items: center;
	}
	.record-status{
		color: #f39f3e;
	}
	.record-doctor-img{
		padding: 30rpx 10rpx;
	}
	
	.record-other{
		color: #808080;
	}
	.record-doctor-img image{
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
		border: 1rpx solid #eaeaea;
		margin-right: 20rpx;
	}
	.record-other *{
		padding: 10rpx 0;
	}
	.record-doctor{
		display: flex;
		align-items: center;
		padding: 0;
	}
	.record-doctor .record-doctor-name{
		font-size: 30rpx;
		font-weight: bold;
		color: #555555;
		margin-right: 20rpx;
	}
	.record-footer-button{
		border: 1rpx solid #28b294;
		color: #28b294;
		border-radius: 10rpx;
		padding: 5rpx 20rpx;
	}
	
	
</style>
