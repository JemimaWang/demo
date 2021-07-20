<!-- 复诊配药 -->
<template>
	<view class="container">
		<view class="avatar">
			<view class="doctor-img">
				<image :src="form.doctor.avatarUrl"></image>
			</view>
			<view>
				<view class="doctor-info">
					<view class="doctor-name">{{form.doctor.doctorName}}</view>
					<view class="level-name">{{form.doctor.levelName}}</view>
				</view>
				<view class="dept-name">{{form.doctor.deptName}}</view>
			</view>
			<view class="select iconfont change-doctor" @tap="toChooseDoctor()">更换医生&#xe60d;</view>
		</view>
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
				<view v-if="!diagnoseModifying" @tap="diagnoseModifying=true" class="form-item-diagnosis">
					{{form.diagnosis}}</view>
				<input v-if="diagnoseModifying" v-model="form.diagnosis" @blur="diagnoseModifying=false"
					class="form-item-diagnosis">
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
			<view class="content" v-if="!descriptionModifying" @tap="descriptionModifying=true">{{form.description}}
			</view>
			<textarea v-if="descriptionModifying" @blur="descriptionModifying=false" v-model="form.description" />
		</view>
		<view class="images">
			<view class="name">病情照片</view>
			<photoPicker @changeImg="changeImg"></photoPicker>
			<view class="tips">
				{{tips}}
			</view>
		</view>
		<view class="deliver-btn">
			<button type="primary" @tap="submit()">提交</button>
		</view>
		<view>
			<uni-popup ref="popup" type="message">
				<uni-popup-message type="error" :message="errorMessage" :duration="2000"></uni-popup-message>
			</uni-popup>
		</view>
	</view>
</template>

<script>
	import tags from "../../components/tags.vue"
	import photoPicker from "../../components/photoPicker.vue"
	export default {
		components: {
			tags,
			photoPicker
		},
		data() {
			return {
				errorMessage: '',
				tips: "上传病情照片、化验单、检查资料、报告单、药品处方单，若为皮肤问题，建议对准患处拍摄。照片仅自己和医生可见",
				diagnoseModifying: false,
				descriptionModifying: false,
				form: {
					diagnosis: '',
					drugIds:[],
					drugNames: [],
					drugs:[],
					description: '从昨天晚上开始腹泻，头晕眼花，伴有呕吐症状，体温39摄氏度',
					imgs: [],
					personShortInfo: '',
					personOtherInfo: '',
					person: null,
					doctor: null,
					user: null
				},


			};
		},
		methods: {
			complete() {
				let _this = this;
				if (_this.form.personShortInfo != '') {
					uni.navigateTo({
						url: "../fillInfo/fillInfo?flag=1&person=" + encodeURIComponent(JSON.stringify(_this.form
							.person)),
					})
				} else {
					uni.navigateTo({
						url: "../fillInfo/fillInfo?flag=0",
					})
				}

			},
			addMedicine() {
				let _this = this;
				uni.navigateTo({
					url: "../addMedicine/addMedicine?selected=" + encodeURIComponent(JSON.stringify(_this.form
						.drugIds)),
				})
			},
			deleteTag(arr) {
				this.form.drugs = arr;
				this.form.drugIds = this.getDrugIds(arr);
				this.form.drugNames = this.getDrugNames(arr);
				// console.log(this.form.drugs)
			},
			getDrugIds(drugs){
				let drugIds = [];
				for(let item of drugs){
					drugIds.push(item.drugId);
				}
				return drugIds;
			},
			getDrugNames(drugs){
				let drugNames = [];
				for(let item of drugs){
					drugNames.push(item.drugName);
				}
				return drugNames;
			},
			toChooseDoctor() {
				uni.navigateTo({
					url: "../chooseDoctor/chooseDoctor"
				})
			},
			changeImg(arr) {
				this.form.imgs = arr;
				console.log(arr);
			},
			submit() {
				if (this.form.person === null) {
					this.errorMessage = '请完善问诊人信息';
					this.$refs.popup.open('top');
					return;
				}
				if (this.form.diagnosis === '') {
					this.errorMessage = '请填写确诊诊断';
					this.$refs.popup.open('top');
					return;
				}
				if (this.form.drugs.length == 0) {
					this.errorMessage = '请选择所需药品';
					this.$refs.popup.open('top');
					return;
				}
				if (this.form.description === '') {
					this.errorMessage = '请填写病情描述';
					this.$refs.popup.open('top');
					return;
				}
				console.log(this.form);
				let form = this.form;
				let genderCode = form.person.sex === '男' ? 1 : 2;
				let consultAsk = {
					orgId: form.doctor.orgId,
					orgName: form.doctor.orgName,
					deptId: form.doctor.deptId,
					deptName: form.doctor.deptName,
					doctorId: form.doctor.doctorId,
					doctorName: form.doctor.doctorName,
					doctorLevelCode: form.doctor.levelCode,
					doctorLevelName: form.doctor.levelName,
					createUserId: form.user.userId,
					personName: form.person.name,
					personCardType: '01',
					personCardId: form.person.identity,
					personGenderCode: genderCode,
					personGenderName: form.person.sex,
					personBirthDate: form.person.birthdate,
					personAge: form.person.age,
					personPhoneNo: form.user.phone,
					question: form.description,
					diagnosis:form.diagnosis,
					drugIds: form.drugIds.toString(),
					drugNames: form.drugNames.toString(),
					photoIds: form.imgs.toString()
				}
				console.log(consultAsk)
				uni.request({
					url:"http://47.102.159.24:9000/addConsult",
					method:"POST",
					data:consultAsk,
					success:res=>{
						console.log(res)
						uni.navigateBack();
					}
				})
			}

		},
		onShow(object) {
			if (!!object) {
				if (object.name === 'fillInfo') {
					this.form.person = object.person;
					this.form.personShortInfo = object.personShortInfo;
					this.form.personOtherInfo = object.personOtherInfo;
				} else if (object.name === "addMedicine") {
					let drugIds = object.selected;
					console.log(this.form.drugs)
					let _this = this;
					uni.request({
						url: "http://47.102.159.24:9000/getDrug",
						success: res => {
							let drugs = res.data.data;
							let drugSelected = [];
							let drugNames = [];
							for(let item of drugs){
								if(drugIds.indexOf(item.drugId)>=0){
									drugNames.push(item.drugName)
									drugSelected.push(item)
								}
							}
							_this.form.drugNames = drugNames;
							_this.form.drugIds = drugIds;
							_this.form.drugs = drugSelected;
						}
					})
					//根据传回的id组合获得药的组合
				} else if (object.name === "chooseDoctor") {
					this.form.doctor = object.doctor
				}

			}
		},
		created() {
			this.form.user = JSON.parse(uni.getStorageSync("user"));
			let _this = this;
			uni.request({
				url: "http://47.102.159.24:9000/getDoctor",
				success: res => {
					console.log(res)
					_this.form.doctor = res.data.data[0]
				}
			})

		}

	}
</script>

<style>
	.avatar {
		display: flex;
		align-items: center;
		border-bottom: 15rpx solid #f2f2f2;
	}

	.doctor-img {
		margin: 20rpx 40rpx;
		margin-right: 20rpx;
		margin-top: 30rpx;
	}

	.doctor-img image {
		border: 1rpx solid #8F8F94;
		width: 150rpx;
		height: 150rpx;
		border-radius: 50%;
	}

	.doctor-info {
		display: flex;
		align-items: center;
	}

	.doctor-name {
		color: #4b4b4b;
		padding: 20rpx 0;
		font-size: 35rpx;
		font-weight: bold;
	}

	.level-name {
		border: 1rpx solid #f2aa77;
		border-radius: 25rpx;
		vertical-align: center;
		font-size: 25rpx;
		line-height: 25rpx;
		height: 30rpx;
		padding: 0 5rpx;
		color: #f2aa77;

	}

	.dept-name {
		font-size: 30rpx;
		padding: 20rpx 0;
		padding-top: 0;
	}

	.change-doctor {
		font-size: 30rpx;
		margin-left: 130rpx;
		color: #cfcfcf;
	}

	.form {
		padding: 0 20rpx;
		border-bottom: 15rpx solid #f2f2f2;
	}

	.form-item {
		font-size: 30rpx;
		display: flex;
		justify-content: space-between;
		padding: 30rpx 30rpx;
		border-bottom: 3rpx solid #f2f2f2;
	}

	.form-blank-item {
		height: 100rpx;
	}

	.form-item-name {
		color: #8F8F94;
	}

	.form-item-not-flex {
		display: block;
	}

	.star {
		color: #f00;
	}

	.form-item-diagnosis {
		width: 200rpx;
		text-align: right;
	}

	.form-item-select-tags {
		display: flex;
		justify-content: space-between;
	}

	.form-item-select {
		font-size: 30rpx;
		color: #cfcfcf;
	}

	.description textarea {
		height: 100rpx;
		padding: 25rpx;
	}

	.name {
		margin: 20rpx 0;
		padding-left: 40rpx;
		border-left: 10rpx solid #30be68;
		font-size: 30rpx;
	}

	.content {
		border-top: 1rpx solid #f2f2f2;
		border-bottom: 15rpx solid #f2f2f2;
		padding: 25rpx 50rpx 40rpx;
		font-size: 30rpx;
	}

	.tips {
		text-indent: 2em;
		width: 600rpx;
		margin: 0 auto;
		margin-top: -30rpx;
		font-size: 25rpx;
		color: #bfbfbf;
	}

	.deliver-btn {
		padding: 100rpx 50rpx 50rpx;
	}
</style>
