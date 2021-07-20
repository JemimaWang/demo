<template>
	<view class="add-medicine">
		<uni-data-checkbox 
			mode="list"
			v-model="selectDrugs"
			multiple 
			:localdata="drugs"></uni-data-checkbox>
		<button style="primary" @tap="saveSelected">保存</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				selectDrugs:[],
				drugs:[],
				drugsTextValue:[]
			}
		},
		methods: {
			saveSelected(){
				let object = {
					name:'addMedicine',
					selected:this.selectDrugs
				};
				console.log(this.selectDrugs)
				var pages = getCurrentPages();
				var prevPage = pages[pages.length - 2]; //上一个页面
				prevPage.onShow(object)
				uni.navigateBack();
			},
			
			getDrugsTextValue(){
				for(let item of this.drugs){
					item.text = item.drugName
					item.value = item.drugId;
				}
			}
		},
		onLoad(options){
			console.log(options)
			if(!!options.selected){
				this.selectDrugs = JSON.parse(decodeURIComponent(options.selected));
				console.log(this.selectDrugs)
			}
		},
		created() {
			let _this = this;
			uni.request({
				url:"http://47.102.159.24:9000/getDrug",
				success:res=>{
					console.log(res)
					_this.drugs = res.data.data
					_this.getDrugsTextValue()
					console.log(_this.drugs)
				}
			})
		}
	}
</script>

<style>
	.add-medicine{
		padding: 50rpx;
	}
</style>
