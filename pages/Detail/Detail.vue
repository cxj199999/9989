<template>
	<view>
		<view>
			<image :src="inf.pic" style="width: 100%;"></image>
		</view>
		<view>
			{{inf.name}}
		</view>
		<view class="">
			{{inf.price}}
		</view>
		<uni-goods-nav style="position: fixed;bottom: 0;width: 100%;" @buttonClick="clickHandle" @click="leftClick"/>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			    inf:{}
			}
		},
		onLoad(option){
			uni.request({
				url: 'http://127.0.0.1:3000/indexList/'+option.id,
				method: 'GET',
				success: res => {
					this.inf = res.data
					// 设置头部导航的名称
					uni.setNavigationBarTitle({
						title:res.data.name
					})
				},
			});
		},
		methods: {
			leftClick(e){
				// 点击了购物车按钮
				if(e.index===1){
					uni.switchTab({
						url:'/pages/cart/cart'
					})
				}
			},
		clickHandle(e){
			console.log(e);
			if(e.index===0){
				const {id,name,price,pic,desc} = this.inf
				// 添加购物车按钮
				// 重组添加到购物车的数据
				let data = {
					id,
					name,
					price,
					pic,
					desc,
					count:1
				}
				uni.request({
					url:'http://localhost:3000/cartList',
					method:'POST',
					data,
					success:(res)=>{
						if(res){
							uni.showToast({
								title:'添加成功'
							})
						}else{
							uni.showToast({
								title:'添加失败',
								icon:'error'
							})
						}
					}
				})
			}
		},
	
		
		
		
		
		
		}
	}
</script>

<style>

</style>
