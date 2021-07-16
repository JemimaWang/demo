<template>
	<view class="add-medicine">
		<uni-data-checkbox 
			mode="list"
			v-model="selectDrugs"
			multiple 
			:localdata="drugs2"></uni-data-checkbox>
		<button style="primary" @tap="saveSelected">保存</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				selectDrugs:[],
				drugs2:[
					{text:'肠炎宁片',value:'肠炎宁片'},
					{text:'连花清瘟胶囊',value:'连花清瘟胶囊'},
					{text:'小柴胡颗粒',value:'小柴胡颗粒'},
					{text:'布洛芬缓释胶囊',value:'布洛芬缓释胶囊'},
					{text:'清凉油(白色)',value:'清凉油(白色)'},
					{text:'曲安奈德益康唑乳膏',value:'曲安奈德益康唑乳膏'},
					{text:'卡托普利片',value:'卡托普利片'},
					{text:'磷酸西格列汀片',value:'磷酸西格列汀片'},
					{text:'盐酸二甲双胍片',value:'盐酸二甲双胍片'},
					{text:'小儿氨酚烷胺颗粒',value:'小儿氨酚烷胺颗粒'},
					{text:'小儿感冒口服液',value:'小儿感冒口服液'},
					{text:'炉甘石洗剂',value:'炉甘石洗剂'},
					{text:'红霉素眼膏',value:'红霉素眼膏'},
					{text:'呋塞米片',value:'呋塞米片'}
				]
			}
		},
		methods: {
			saveSelected(){
				let object = {
					name:'addMedicine',
					selected:this.selectDrugs
				};
				var pages = getCurrentPages();
				var prevPage = pages[pages.length - 2]; //上一个页面
				prevPage.onShow(object)
				uni.navigateBack();
			}
		},
		onLoad(options){
			console.log(options)
			if(!!options.selected){
				this.selectDrugs = JSON.parse(decodeURIComponent(options.selected));
				console.log(this.selectDrugs)
			}
		}
	}
</script>

<style>
	.add-medicine{
		padding: 50rpx;
	}
</style>
