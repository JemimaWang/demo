<template>
	<view class="container">
		<avatar></avatar>
		<view class="form">
			<view class="form-item" @tap="complete()">
				<view class="form-item-name"><text class="star">*</text>问诊人</view>
				<view>{{form.personShortInfo}}</view>
			</view>
			<view class="form-item form-blank-item" @tap="complete()">
				<text v-if="form.personShortInfo!=''">{{form.personOtherInfo}}</text>
			</view>
			<view class="form-item">
				<view class="form-item-name"><text class="star">*</text>确诊诊断</view>
				<view v-if="!diagnoseModifying" @tap="diagnoseModifying=true">{{form.diagnose}}</view>
				<input v-if="diagnoseModifying" v-model="form.diagnose" @blur="diagnoseModifying=false">
			</view>
			<view class="form-item form-item-not-flex">
				<view class="form-item-select-tags">
					<view class="form-item-name"><text class="star">*</text>所需药品</view>
					<view class="form-item-select iconfont" @click="addMedicine()">添加药品&#xe60d;</view>
				</view>
				<view class="form-item-tags">
					<tags :tagArr="form.drugs" @deleteTag="deleteTag"></tags>
				</view>
			</view>
		</view>
		<view class="description">
			<view class="name"><text class="star">*</text>病情描述</view>
			<view class="content" v-if="!descriptionModifying" @tap="descriptionModifying=true">{{form.description}}</view>
			<textarea
				v-if="descriptionModifying" @blur="descriptionModifying=false"
				v-model="form.description" />
		</view>
		<view class="images">
			<view class="name">病情照片</view>
			<photoPicker></photoPicker>
			<view class="tips">
				{{tips}}
			</view>
		</view>
		<view class="deliver-btn">
			<button type="primary">提交</button>
		</view>
		
		
	</view>
</template>

<script>
	import avatar from "../../components/avatar.vue"
	import tags from "../../components/tags.vue"
	import photoPicker from "../../components/photoPicker.vue"
	export default {
		components:{
			tags,avatar,photoPicker
		},
		data() {
			return {
				tips:"上传病情照片、化验单、检查资料、报告单、药品处方单，若为皮肤问题，建议对准患处拍摄。照片仅自己和医生可见",
				diagnoseModifying:false,
				descriptionModifying:false,
				form:{
					diagnose:'霍乱',
					drugs:['肠炎宁片','连花清瘟胶囊'],
					description:'从昨天晚上开始腹泻，头晕眼花，伴有呕吐症状，体温39摄氏度',
					imgs:[require('../../static/logo.png'),require('../../static/logo.png')],
					personShortInfo:'',
					personOtherInfo:'',
					person:{}
				},
				
			};
		},
		methods:{
			complete(){
				let _this = this;
				if(_this.form.personShortInfo!=''){
					uni.navigateTo({
						url:"../fillInfo/fillInfo?flag=1&person="+encodeURIComponent(JSON.stringify(_this.form.person)),
					})
				}else{
					uni.navigateTo({
						url:"../fillInfo/fillInfo?flag=0",
					})
				}
				
			},
			addMedicine(){
				let _this = this;
				uni.navigateTo({
					url:"../addMedicine/addMedicine?selected="+encodeURIComponent(JSON.stringify(_this.form.drugs)),
				})
			},
			deleteTag(arr){
				// console.log(arr)
				this.form.drugs = arr;
				// console.log(this.form.drugs)
			}
		},
		onShow(object) {
			if(!!object){
				if(object.name==='fillInfo'){
					this.form.person = object.person;
					this.form.personShortInfo = object.personShortInfo;
					this.form.personOtherInfo = object.personOtherInfo;
				}else if(object.name==="addMedicine"){
					this.form.drugs = object.selected
					console.log(this.form.drugs)
				}
				
			}
		}
	}
</script>

<style>
	.form{
		padding: 0 20rpx;
		border-bottom: 15rpx solid #f2f2f2;
	}
	.form-item{
		font-size: 30rpx;
		display: flex;
		justify-content: space-between;
		padding: 30rpx 30rpx;
		border-bottom: 3rpx solid #f2f2f2;
	}
	.form-blank-item{
		height: 100rpx;
	}
	.form-item-name{
		color: #8F8F94;
	}
	.form-item-not-flex{
		display: block;
	}
	
	.star{
		color: #f00;
	}
	.form-item-select-tags{
		display: flex;
		justify-content: space-between;
	}
	
	.form-item-select{
		font-size: 30rpx;
		color: #cfcfcf;
	}
	.description textarea{
		height: 100rpx;
		padding: 25rpx;
	}
	.name{
		margin: 20rpx 0;
		padding-left: 40rpx;
		border-left:10rpx solid  #30be68;
		font-size: 30rpx;
	}
	.content{
		border-top: 1rpx solid #f2f2f2;
		border-bottom: 15rpx solid #f2f2f2;
		padding: 25rpx 50rpx 40rpx;
		font-size: 30rpx;
	}
	.tips{
		text-indent:2em;
		width: 600rpx;
		margin: 0 auto;
		margin-top: -30rpx;
		font-size: 25rpx;
		color: #bfbfbf;
	}
	.deliver-btn{
		padding: 100rpx 50rpx 50rpx;
	}

</style>
