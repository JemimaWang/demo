<template>
	<view class="bind-phone">
		<view class="bind-tips">{{tips}}</view>
		<uni-forms ref="form"  :modelValue="user" v-if="user!=null">
			<uni-forms-item  name="phone">
				<uni-easyinput  v-model="user.phone" placeholder="请填写" />
			</uni-forms-item>
		</uni-forms>
		<button type="primary" @tap="bindPhone()">{{getBtnTxt()}}</button>
		<view>
			<uni-popup ref="popup" type="message">
			    <uni-popup-message type="error" message="该手机号已被绑定" :duration="2000"></uni-popup-message>
			</uni-popup> 
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				user:null,
				phone:'',
				tips:''
			}
		},
		methods: {
			getTips(){
				let phone = this.user.phone;
				if(phone===''){
					return '你尚未绑定手机号';
				}else{
					return '你已绑定手机号：'+phone;
				}
			},
			getBtnTxt(){
				let phone = this.user.phone;
				if(phone===''){
					return '绑定手机号';
				}else{
					return '换绑手机号';
				}
			},
			async bindPhone(){
				let _this = this;
				//待修改
				//把更改的user存入数据库
				var [error, res] = await uni.request({
					//判断是否存在该手机号
					url: "http://47.102.159.24:9000/selectAccoutByPhone",
					data:{
						phoneNo:_this.user.phone
					}
				})
				console.log(res);
				if(res.data.data!=null&&res.data.data!=undefined){
					//弹出报错弹窗
					this.$refs.popup.open('top');
					return;
				}
				console.log(_this.user)
				let user2 = {
					userType: _this.user.type,
					miniOpenId: _this.user.openId,
					phoneNo: _this.user.phone,
					createTime:_this.user.createTime,
					userId:_this.userId
				}
				console.log(user2)
				uni.request({
					url:"http://47.102.159.24:9000/editPersonAccout",
					method:"POST",
					data:{
						userType: _this.user.type,
						miniOpenId: _this.user.openId,
						phoneNo: _this.user.phone,
						createTime:_this.user.createTime,
						userId:_this.user.userId
					},
					success:res=>{
						console.log(res)
						uni.setStorageSync("user",JSON.stringify(_this.user));
						uni.navigateBack();
					}
				})
			},
		},
		created() {	
			this.user = JSON.parse(uni.getStorageSync("user"));
			this.tips = this.getTips()
			console.log(this.user);
		}
	}
</script>

<style>
	.bind-phone{
		
	}
	.bind-tips{
		text-align: center;
		color: #808080;
		padding: 50rpx;
	}
	.bind-phone .uni-forms{
		margin: 30rpx;
	}
	.bind-phone button{
		position: absolute;
		width: 500rpx;
		top: 80%;
		left: 50%;
		transform: translate(-50%,-50%);
	}

</style>
