<template>
	<view>
		<view class="prescrption-header">
			<view>{{prescrption.org.orgName}}</view>
			<view>处方笺</view>
			<view class="prescrption-type">{{prescrption.type}}</view>
		</view>
		<view class="person-content">
			<view>
				<view>姓名 {{prescrption.person.name}}</view>
				<view>性别 {{prescrption.person.sex}}</view>
				<view>年龄 {{prescrption.person.age}}岁</view>
				<view>日期 {{getFormatDate(prescrption.createTime)}}</view>
			</view>
			<view>
				<view>身份证号 {{prescrption.person.identity}}</view>
				<view>手机号 {{prescrption.person.phone}}</view>
			</view>
		</view>
		<view class="drug-content">
			<view class="drug-header">Rp</view>
			<view class="drug-item" v-for="(item,index) in prescrption.drugs">
				<view>
					<view class="drug-item-name">{{item.drugName}}</view>
					<view class="drug-item-usage">
						用法：{{item.dose+item.doseUnit}}/次 {{item.frequencyName}} {{item.usageName}} 
					</view>
				</view>
				<view>{{item.quantity+item.packUnit}}</view>
			</view>
			<view class="drug-prize">药费：<text>¥{{getTotalPrice()}}</text>元</view>
		</view>
		<view class="prescrption-footer">
			<view class="doctor-content">
				处方医师：{{prescrption.doctor.doctorName}}<br>
				审核医师：<br>
				发药医师：
			</view>
			<view>盖章：</view>
		</view>
		<view class="prescrption-tips">{{tips}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tips:'*药师温馨提示：请遵医嘱服药！处方当日有效！',
				prescrption:null
			}
		},
		methods: {
			getFormatDate(time){
				let date = new Date(time)
				return date.format('yyyy-MM-dd')
			},
			getTotalPrice(){
				let total = 0.00;
				for(let item of this.prescrption.drugs){
					total = total+item.price;
				}
				return total.toFixed(2);
			},
		},
		onLoad(options) {
			console.log(options)
			let consultAsk = JSON.parse(decodeURIComponent(options.consultAsk))
			console.log(consultAsk)
			let _this = this;
			uni.request({
				url:"http://47.102.159.24:9000/selectPrescriptionByConsult",
				data:{
					consultId:consultAsk.consultId
				},
				success:res=>{
					console.log(res)
					let prescriptionInfo = res.data.data;
					let prescription = {
						createTime:prescriptionInfo.createTime,
						type:prescriptionInfo.prescrptionType,
						org:{
							orgId:consultAsk.orgId,
							orgName:consultAsk.orgName
						},
						doctor:{
							doctorId:consultAsk.doctorId,
							doctorName:consultAsk.doctorName
						},
						person:{
							name:consultAsk.personName,//person_name(consult_ask)
							sex:consultAsk.personGenderName,//person_gender_name(consult_ask)
							age:consultAsk.personAge,//person_age(consult_ask)
							phone:consultAsk.personPhoneNo,//person_phone_no(consult_ask)
							identity:consultAsk.personCardId,//person_card_id(consult_ask)
						},
					}
					uni.request({
						url:"http://47.102.159.24:9000/selectPrescriptionDrugByPrescription",
						data:{
							prescriptionId:prescriptionInfo.prescriptionId
						},
						success:res2=>{
							prescription.drugs = res2.data.data;
							_this.prescrption = prescription
						}
					});
					
				}
			})
		}
	}
</script>

<style>
	.prescrption-header{
		font-size: 50rpx;
		text-align: center;
		padding: 50rpx;
		position: relative;
	}
	.prescrption-type{
		font-size: 30rpx;
		position: absolute;
		width: 4em;
		border: 1rpx solid #333333;
		top: 60%;
		left: 80%;
	}
	.person-content{
		padding: 40rpx 20rpx 100rpx;
		
	}
	.person-content>view{
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 20rpx;
	}
	.drug-content{
		border-top: #000 10rpx solid;
		padding: 30rpx 40rpx 50rpx;
		font-size: 30rpx;
	}
	.drug-header{
		font-size: 40rpx;
	}
	.drug-item{
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin: 30rpx 0;
		padding-bottom: 0;
		color: #585858;
	}
	.drug-prize{
		text-align: right;
	}
	.drug-prize text{
		color: #e6895c;
	}
	.drug-item-usage{
		color: #b1b1b1;
	}
	.prescrption-footer{
		display: flex;
		justify-content: space-between;
		padding-left: 20rpx;
		padding-right: 100rpx;
	}
	.doctor-content{
		line-height: 50rpx;
	}
	.prescrption-tips{
		color: #b1b1b1;
		text-align: center;
		padding: 50rpx 0 150rpx;
	}
	
</style>
