<template>
	<view class="home">
		<view class="scrollNav">
			<scroll-view scroll-x class="navscroll">
				<view class="item" :class="index === navIndex?'active':''" v-for="(item,index) in navArr" :key="item.id"
					@click="clickNav(index,item.id)">
					{{item.classname}}
				</view>
			</scroll-view>
		</view>
		<view class="content">
			<view class="row" v-for="(item,index) in newsArr" :key="index">
				<newsBox @click.native="goDetail(item)" :item="item"></newsBox>
			</view>
		</view>

		<view class="nodata" v-if="!newsArr.length">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>
		<view class="loading" v-if="newsArr.length">
			<view v-show="loading===1">
				数据加载中...
			</view>
			<view v-show="loading===2">
				没有更多了~
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [],
				newsArr: [],
				currentPage: 1,
				currentId: 50,
				loading: 0
			}
		},
		onLoad() {
			this.getNavData();
			this.getNewsData();
		},
		onReachBottom() {
			if (this.loading === 2) {
				return
			}
			this.currentPage++;
			this.loading = 1;
			this.getNewsData();
		},
		methods: {
			clickNav(index, id) {
				this.navIndex = index;
				this.currentPage = 1;
				this.currentId = id;
				this.loading = 0;
				this.newsArr = []
				this.getNewsData(id)
			},
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			getNavData() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: res => {
						this.navArr = res.data
					}
				})
			},
			getNewsData(id) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						cid: this.currentId,
						page: this.currentPage
					},
					success: res => {
						res.data.length === 0 && (this.loading = 2);
						this.newsArr = [...this.newsArr, ...res.data]
					}
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.navscroll {
		height: 100rpx;
		background-color: #F7F8FA;
		white-space: nowrap;
		position: fixed;
		top: var(--window-top);
		left: 0;
		z-index: 10;

		/deep/ ::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}

		.item {
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;

			&.active {
				color: #31C27C;
			}
		}
	}

	.content {
		padding: 0 30rpx;
		padding-top: 130rpx;

		.row {
			border-bottom: 1px dashed #EFEFEF;
			padding: 20rpx 0;
		}

	}

	.nodata {
		display: flex;
		justify-content: center;

		image {
			width: 360rpx;
		}
	}

	.loading {
		text-align: center;
		font-size: 26rpx;
		color: #888;
		line-height: 2em;
	}
</style>