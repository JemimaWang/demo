<template>
	<view class="fill-info">
		 <uni-forms :modelValue="formData" ref="form">
			<uni-forms-item label="姓名" name="name" required>
				<uni-easyinput type="text" v-model="formData.name" placeholder="请填写" />
			</uni-forms-item>
			<uni-forms-item label="身份证号" name="identity" required>
				<uni-easyinput type="text" v-model="formData.identity" placeholder="请填写" />
			</uni-forms-item>
			<uni-forms-item label="性别" name="sex" required>
				<uni-data-checkbox v-model="formData.sex" :localdata="sexData">
				</uni-data-checkbox>
			</uni-forms-item>
			<uni-forms-item label="出生日期" name="birthdate" required>
				<uni-datetime-picker 
					type="date" v-model="formData.birthdate" 
					start="1900-06-01 06:30:30" end="2030-6-1" 
					return-type="timestamp">
				</uni-datetime-picker> 
			</uni-forms-item>
			<uni-forms-item label="手机号" name="phone" required>
				<uni-easyinput type="text" v-model="formData.phone" placeholder="请填写" />
			</uni-forms-item>
			 
		</uni-forms>
		<button type="primary" @tap="submit()">保存</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				formData:{
					name:'',
					identity:'',
					sex:'',
					sexCode:'',
					birthdate:'',
					phone:'',
					age:0,
				},
				sexData: [{
					text: '男',
					value: '男'
				}, {
					text: '女',
					value: '女'
				}],
				rules:{
					name:{
						rules:[
							{required:true,errorMessage: '请输入姓名'},
							{minLength: 2,maxLength: 15,errorMessage: '姓名长度在 {minLength} 到 {maxLength} 个字符'}
						]
					},
					sex:{
						rules:[
							{required:true,errorMessage: '请选择性别'}
						]
					},
					identity:{
						rules:[
							{required:true,errorMessage: '请输入身份证'},
							{maxLength:18,minLength:18,errorMessage:'请输入正确格式的身份证：18位'}
						]
					},
					phone:{
						rules:[
							{required:true,errorMessage: '请输入手机号'},
							{maxlength:11,minLength:11,errorMessage:'请输入正确格式的手机号：11位'}
						]
					},
					birthdate:{
						rules:[
							{required:true,errorMessage: '请选择出生日期'},
						]
					}
				}
			}
		},
		methods: {
			submit(){
				let _this = this;
				this.$refs.form.submit().then(res=>{
					//表单验证成功
					console.log('表单数据信息：', res);
					this.formData.age = this.getAge();
					if(this.formData.sex==='男'){
						this.formData.sexCode = 1;
					}else{
						this.formData.sexCode = 2;
					}
					let personShortInfo = this.formData.name+' '+this.formData.sex+' '+this.formData.age+'岁'
					let str = this.formData.phone.slice(0,3)+'****'+this.formData.phone.slice(7)
					let str2 = this.formData.identity.slice(0,6)+'********'+this.formData.identity.slice(14)
					let personOtherInfo = str + '， '+str2
					//返回
					var pages = getCurrentPages();
					var prevPage = pages[pages.length - 2]; //上一个页面
					console.log(prevPage)
					console.log(this.formData)
					let object = {
						name:'fillInfo',
						personShortInfo:personShortInfo,
						personOtherInfo:personOtherInfo,
						person:this.formData,
					}
					prevPage.onShow(object)
					uni.navigateBack();
					
				}).catch(err =>{
					console.log('表单错误信息：', err);
				})
			},
			getAge(){
				//根据出生日期计算年了
				let now = new Date()
				let birth = new Date(this.formData.birthdate);
				let substract = now.getYear()-birth.getYear();
				if(now.getMonth()<birth.getMonth()){
					substract--;
				}
				console.log(substract)
				return substract
			}
			
		},
			
		onReady(){
			this.$refs.form.setRules(this.rules)
		},
		onLoad(options) {
			console.log(options)
			if(options.flag==="1"){
				this.formData = JSON.parse(decodeURIComponent(options.person));
				console.log(this.formData)
			}
		}
	}
</script>

<style>
	.fill-info{
		padding: 30rpx;
	}
</style>
