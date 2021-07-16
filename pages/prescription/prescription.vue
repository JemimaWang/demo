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
				<view>日期 {{prescrption.createTime.format('yyyy-MM-dd')}}</view>
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
						用法：{{item.dose+item.doseUnit}}/次 {{item.frequencyAbbr}} {{item.usageName}} 
					</view>
				</view>
				<view>{{item.quantity+item.packUnit}}</view>
			</view>
			<view class="drug-prize">药费：<text>¥{{getTotalPrize()}}</text>元</view>
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
				prescrption:{
					createTime:new Date(),//create_time(prescription_info)
					type:'普通药品处方',//处方类型
					org:{
						orgId:1,
						orgName:'创业慧康医院',//org_name(consult_ask)
					},
					doctor:{
						doctorId:1,
						doctorName:'许武林'
					},
					person:{
						name:'童晓航',//person_name(consult_ask)
						sex:'男',//person_gender_name(consult_ask)
						age:29,//person_age(consult_ask)
						phone:'15306518065',//person_phone_no(consult_ask)
						identity:'330781199112182333',//person_card_id(consult_ask)
						
					},
					drugs:[{
						drugName:'莲花清瘟胶囊',//药品名字
						specification:'0.35g*36粒',//药品规格
						usageName:'口服',//药品用法
						dose:'0.35',//一次剂量
						doseUnit:'g',
						frequencyAbbr:'qd',//用药频率缩写
						prize:29.10,//药品单价
						quantity:1.00,//药品数量
						packUnit:'盒',//包装单位
						sortNumber:1,//顺序号
						groupNumber:1,
						remark:'',//嘱托
					},
					{
						drugName:'小柴胡颗粒',//药品名字
						specification:'10g*9袋',//药品规格
						usageName:'口服',//药品用法
						dose:'10.00',//一次剂量
						doseUnit:'g',
						frequencyAbbr:'qd',//用药频率缩写
						prize:18.20,//药品单价
						quantity:1.00,//药品数量
						packUnit:'盒',//包装单位
						sortNumber:1,//顺序号
						groupNumber:1,
						remark:'',//嘱托
					}]
					
				},
			}
		},
		methods: {
			getTotalPrize(){
				let total = 0.00;
				for(let item of this.prescrption.drugs){
					total = total+item.prize;
				}
				return total.toFixed(2);
			}
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
