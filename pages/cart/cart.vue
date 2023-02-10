<template>
	<view>
		<!-- 购物车详情 -->
		<view
			v-for="(item, index) in cartList"
			:key="item.id"
			style="display: flex;justify-content: space-evenly;background-color: white;margin-left: 20rpx;margin-right:20rpx;border-radius: 10px;margin-top: 30rpx;"
		>
		   <!-- 选择选项 -->
			<view class="" style="line-height: 200rpx;">
				<label>
					<checkbox-group @change="checkGoods">
					<checkbox
											:value="item.id.toString()"
											
											:checked="buyList.includes(item.id)"
											color="red"
										/>
					</checkbox-group>
					</label>
			</view>
			<!-- tu片 -->
			<view>
				<image
					:src="item.pic"
					style="width: 180rpx;height: 180rpx;"
				></image>
			</view>
			<!-- 图片信息 -->
			<view class="">
				<view style="font-size:1.1em;">{{ item.name }}</view>
				<view style="font-size: 1.3em;color:red;font-weight: red;">
					¥{{ item.price }}
				</view>

				<!-- 按钮 -->
				<view style="display: flex;justify-content:flex-end">
					<view
						style="width: 50rpx;height: 50rpx;border: 1px solid gray;border-radius: 20%;text-align: center;"
						@click="changeCount(-1, item, index)"
					>
						－
					</view>
					<span style="margin: 15rpx; margin-bottom: -40rpx;">
						{{ item.count }}
					</span>
					<view
						style="width: 50rpx;height: 50rpx;border: 1px solid gray;border-radius: 20%;text-align: center; ;"
						@click="changeCount(1, item, index)"
					>
						＋
					</view>
				</view>
			</view>
		</view>

		<!-- dibu底部购物车 -->
		<view class="uni-container">
			<uni-goods-nav
				@click="onClick(item.path)"
				v-for="item in options"
			/>
		</view>
	</view>
</template>

<script>
import url from '../../config/url.js';
export default {
	data() {
		return {
			options: [{ path: '/pages/address/address' }],
			indexList: [],
			cartList: [],
			buyList: [],
		};
	},
	onShow() {
		console.log('显示');
		uni.request({
			url: 'http://localhost:3000/cartList',
			success: res => {
				console.log(res);
				this.cartList = res.data;
			}
		});
	},

	methods: {
		// 购物车跳转
		onClick(url) {
			console.log('点击菜单了');
			console.log(url);
			uni.navigateTo({
				url
			});
		},
		// xuan选择框
			checkGoods(value) {
					console.log(value)
					this.buyList.push(value)
		},
		// 
		changeCount(delta, item, index) {
					// 本地修改
					item.count += delta
					// 判断商品的count数量为0时,就删除数据
					if (item.count <= 0) {
						uni.showModal({
							content: '要让我骂你吗',
							success: (res)=>{
								if (res.confirm) {
									uni.request({
										url:
											'http://localhost:3000/cartList/' + item.id,
										method: 'DELETE',
										success: res => {
											// console.log(res)
											this.cartList.splice(index, 1)
										}
									})
								} else if (res.cancel) {
									item.count = 1
									uni.showToast({
										title: '算你识相'
									})
								}
							}
						})
						return
					}
					// 请求服务端记录数量
					uni.request({
						url: 'http://localhost:3000/cartList/' + item.id,
						method: 'PUT',
						data: item,
						success: res => {
							console.log(res)
						}
					})
				}
	}
};
</script>

<style scoped>
	.uni-container{position: absolute; bottom: 22.5rpx; width: 100%;}
</style>
