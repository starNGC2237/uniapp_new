<template>
	<view class="newsbox">
		<view class="pic">
			<image :src="item.picurl" mode="aspectFill"></image>
		</view>
		<view class="text">
			<view class="title">
				{{item.title}}
			</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>{{item.hits}}浏览</text>
			</view>
			<view class="info" v-if="item.looktime">
				<text>浏览时间：{{formatTime(item.looktime)}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	const dayjs = require('dayjs');
	export default {
		name: "newsBox",
		props:{
			item:{
				type:Object,
				default(){
					return {
						title:"默认标题",
						author:'张三',
						hits:0,
						picurl:"../../static/images/nopic.jpg"
					}
				}
			}
		},
		data() {
			return {
				
			};
		},
		methods:{
			formatTime(time){
				if(String(time).length === 10){
					return dayjs.unix(time).format('YYYY-MM-DD HH:mm:ss');
				}
				if(String(time).length === 13){
					return dayjs(time).format('YYYY-MM-DD HH:mm:ss');
				}
				return time
			}
		}
	}
</script>

<style lang="scss">
	.newsbox {
		display: flex;

		.pic {
			width: 230rpx;
			height: 160rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.text {
			flex: 1;
			padding-left: 20rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;

			.title {
				font-size: 36rpx;
				color: #333;
				word-break: break-all;
				overflow: hidden;
				display: -webkit-box;
				-webkit-line-clamp: 2;
				-webkit-box-orient: vertical;
			}

			.info {
				font-size: 26rpx;
				color: #999;

				text {
					padding-right: 30rpx;
				}
			}
		}
	}
</style>