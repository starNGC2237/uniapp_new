<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/history.png" mode=""></image>
			<view class="text">
				浏览历史
			</view>
		</view>
		<view class="content">
			<view class="row" v-for="(item,index) in listArr" :key="index">
				<newsBox :item="item" @click.native="goDetail(item)"></newsBox>
			</view>
		</view>
		<view class="nohistory" v-if="listArr.length === 0">
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<view class="text">
				暂无浏览历史
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr:[]
			};
		},
		onShow() {
			this.getData()
		},
		methods:{
			goDetail(item){
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			getData(){
				let historyArr = uni.getStorageSync('historyArr') || [];
				this.listArr = historyArr
			}
		}
	}
</script>

<style lang="scss">
	.user {
		.top {
			padding: 50rpx 0;
			background: #F8F8F8;
			color: #666;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			image {
				width: 150rpx;
				height: 150rpx;
			}

			.text {
				font-size: 38rpx;

			}
		}
		.content {
			padding: 0 30rpx;
		
			.row {
				border-bottom: 1px dashed #EFEFEF;
				padding: 20rpx 0;
			}
		}
		.nohistory{
			flex-direction: column;
			justify-content: center;
			align-items: center;
			display: flex;
			image{
				width: 400rpx;
				
			}
			.text{
				font-size: 26rpx;
				color: #888;
			}
		}
	}
	
</style>