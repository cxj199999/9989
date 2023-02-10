<template>
	<view class="">
		<!-- 回到顶部按钮 -->
		<button
			class="mini-btn"
			type="warn"
			size="mini"
			v-show="top"
			@click="gotop"
		>
			<uni-icons type="top" size="20" color="#ccc"></uni-icons>
		</button>
		<!-- 搜索 -->
		<uni-search-bar
			@confirm="search"
			:focus="true"
			v-model="searchValue"
			placeholder="搜索"
			bgColor="#ccc"
		></uni-search-bar>
		<!-- 通告 -->
		<uni-notice-bar
			show-icon
			backgroundColor="#ccc"
			color="white"
			showClose
			scrollable
			showGetMore
			moreText="更多"
			@getmore="getMore"
			single
			text= "离开了思考地方了"
			moreColor="white"
		></uni-notice-bar>

		<!--  -->

		<!-- 轮播 -->
		<swiper
			:indicator-dots="true"
			:autoplay="true"
			:interval="2000"
			:duration="1000"
			style="height: 400rpx;"
		>
			<swiper-item v-for="item in banner" :key="item.id">
				<view class="swiper-item">
					<image :src="item.pic" style="width: 750rpx; "></image>
				</view>
			</swiper-item>
		</swiper>

		<!-- 商品格 -->
		<uni-grid
			:column="2"
			:showBorder="false"
			:square="false"
			style="background-color: #f8f6f7;"
		>
			<uni-grid-item
				v-for="item in indexList"
				:key="item.id"
				style="background-color: white;border-radius: 10px;width:360rpx;margin-left: 8rpx;margin-top: 10rpx;overflow: hidden;"
			>
				<image
					:src="item.pic"
					style="width: 360rpx"
					@click="goDetail(item.id)"
				
					
				></image>
				<view style="padding: 15rpx;margin-top: 10rpx;">
					<view style="font-size: 1.1em;">{{ item.name }}</view>
					<view style="font-size: 0.9em;color:gray">
						{{ item.desc }}
					</view>
					<view style="display: flex;justify-content: space-between;">
						<view
							style="font-size: 1.2em;color:red;font-weight: bold;"
						>
							¥{{ item.price }}
						</view>
						<view>{{ item.buyCount }}件</view>
					</view>
				</view>
			</uni-grid-item>
		</uni-grid>

		<!-- 底部加载 -->
		<uni-load-more
			status="nomore"
			:contentText="{
				contentdown: '滚动加载更多',
				contentrefresh: '正在加载...',
				contentnomore: '暂时没有更多了'
			}"
		></uni-load-more>
	</view>
</template>

<script>
import url from '../../config/url.js';
export default {
	data() {
		return {
			// 搜索
			searchValue: '',
			// 轮播
			banner: [
				{
					id: 1,
					pic:
						'https://wx4.sinaimg.cn/orj360/006d49NDgy1h83bzn9jrlj30v90v915l.jpg',
					co: '刘旭微也是'
				},
				{
					id: 2,
					pic:
						'https://wx3.sinaimg.cn/orj360/006d49NDgy1h83bzo0gqbj30v90v9471.jpg',
					co: '刘旭微也是'
				},
				{
					id: 3,
					pic:
						'https://wx2.sinaimg.cn/orj360/006d49NDgy1h83bzotq62j30v90v9wli.jpg'
					// co:'刘旭微也是''
				},
				{
					id: 4,
					pic:
						'https://wx4.sinaimg.cn/orj360/006d49NDgy1h83bzpdzc9j30v90v9459.jpg'
					// co:'刘旭微也是''
				},
				{
					id: 5,
					pic:
						'https://wx3.sinaimg.cn/orj360/006d49NDgy1h83bzm29p3j30v90v9tid.jpg'
					// co:'刘旭微也是''
				}
			],

			// 商品
			indexList: [],
			// 商品页
			page: 1,
			top: false,
			// loadStatus: 'noMore',
		};
	},

	//   商品加载
	onLoad(option) {
		this.getIndexList();
	},
	onReachBottom() {
		// 获取后续页码的数据
		this.getIndexList(this.page);
	},
	// g滚动距离
	onPageScroll(event) {
		// console.log(event);
		this.top = event.scrollTop > 1000;
	},
	// xial下拉刷新
	onPullDownRefresh() {
		// 下拉刷新之前要清空之前的列表数据
		this.indexList = [];
		this.getIndexList(1, true);
		// 关闭下拉状态
		uni.stopPullDownRefresh();
	},

	methods: {
		// 滚动到页面的位置
		gotop() {
			uni.pageScrollTo({
				// 顶部距离为0 回到顶部
				// 距顶部的距离
				scrollTop: 0,
				duration: 0
			});
		},
		// shang商品获取

		// 商品页加载
		getIndexList(page = 1,isPullDown=false) {
			if(!isPullDown){
							// 判断状态已经为没有更多 就退出请求
							if (this.loadStatus === 'noMore'){
								return
							} 
						}
						this.loadStatus = 'loading'
			uni.request({
				url: url.indexList,
				data: {
					_page: page,
					_limit: 4
				},
				success: res => {
					console.log(res);
					// this.indexList = res.data
					// 新旧数据
					this.indexList = [...this.indexList, ...res.data];
					// yeshu
					// this.$store.commit('saveIndexList', this.indexList);
					// // 存储数据到本地存储 永久存储
					// uni.setStorageSync('indexList', this.indexList);
					this.page++;
				}
			});
		},

		// 搜索
		search() {
			console.log('搜索：' + this.searchValue);
			uni.navigateTo({
				url: '/pages/index/search?keywords=' + this.searchValue
			});
		},

		// 商品跳转
		goDetail(id){
			uni.navigateTo({
				url:'/pages/Detail/Detail?id='+id
			})
		},
		// changeMenu(url) {
		// 	console.log('点击菜单了');
		// 	console.log(url);
		// 	uni.navigateTo({
		// 		url
		// 	});
		// },

		getMore() {
			uni.showToast({
				title: '你想得美'
			});
		}
	}
};
</script>

<style lang="scss">
uni-swiper-item {
	border-radius: 20px;
}
.mini-btn {
	border-radius: 100px;
	width: 2rem;
	height: 35px;
	line-height: 20px;
	display: flex;
	justify-content: center;
	align-items: center;
	position: fixed;
	right: 30rpx;
	bottom: 250rpx;
	z-index: 1000;
}
</style>
