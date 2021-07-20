<template>
	<view class="tags">
		<view v-for="(item,index) in tagArr" class="tag-item":key>
			<view class="tag-close iconfont icon-ziyuan"
				@tap="deleteTag(index)"></view>
			<view class="tag-content">{{item.drugName}}</view>
		</view>
		<view v-if="tagArr.length===0" class="tag-show-empty">
			暂无药品
		</view>
	</view>
</template>

<script>
	export default {
		name:"tags",
		props:['tagArr'],
		data() {
			
			return {
				
			};
		},
		methods:{
			deleteTag(index){
				let arr = this.tagArr;
				for(let i = index; i < arr.length-1; i++){
					arr[i] = arr[i+1]
				}
				arr.pop();
				this.$emit("deleteTag",arr);
			}
		},
		created(){
			console.log(this.tagArr)
			
		}
	}
</script>

<style>
	.tags{
		display: flex;
		flex-wrap: wrap;
	}
	.tag-item{
		margin: 20rpx;
	}
	.tag-close{
		color: #FF0000;
		font-size: 30rpx;
		margin-left: 80rpx;
		
	}
	.tag-content{
		font-size: 25rpx;
		padding: 10rpx;
		width: 100rpx;
		text-align: center;
		background-color: #eaf9f4;
		color: #58c08a;
		margin-top: -20rpx;
	}
	.tag-show-empty{
		padding: 25rpx 20rpx;
		color: #cfcfcf;
	}
</style>
