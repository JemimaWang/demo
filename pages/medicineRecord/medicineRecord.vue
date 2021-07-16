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
					<view class="record-ill">病情：{{item.ill}}</view>
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
				records:[
					{
						consultId:1,//问诊id，数据库consult_id
						createTime:new Date(),//问诊时间-创建时间 数据库create_time 之后通过getAcceptTime转换为格式：2021-05-24 11:26
						doctorImg:'https://qbkeass.cn/fzpy/image/doctors/doctor.png',//医生头像
						doctorName:'林玉娇',//医生姓名 doctor_name
						personName:'张伟',//就诊人-配药人姓名，person_name
						ill:'流行性感冒',//病情-诊断小结,diagnosis
						consultStatus:1,//待完成&已完成 通过consult_status,1待接诊，2进行中，3已完成
					},
					{
						consultId:2,
						createTime:new Date(),//问诊时间(接诊时间) 格式：2021-05-24 11:26
						doctorImg:'https://qbkeass.cn/fzpy/image/doctors/doctor.png',
						doctorName:'林玉娇',//医生
						personName:'张伟',//就诊人
						ill:'流行性感冒',//病情
						consultStatus:3
					}
				]
			}
		},
		methods: {
			getCreateTime(index){
				let time = this.records[index].createTime;
				if(time===null){
					return '';
				}else{
					return time.format('yyyy-MM-dd hh:mm');
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
					//url:"../prescription/prescription?consultId="+this.durgs[index].consultId
					url:"../prescription/prescription"
				})
			}
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
