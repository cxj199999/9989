<template>
	<!-- 登录进来 -->
	<view>
	<view
				v-if="user" 
				style="display: flex;flex-direction: column;align-items: center; "
				
			>
				<view style="margin-top: 80rpx;">
					<image
						src="/static/005TyZEcgy1gx6r4lmar5j30u00vtwfw.jpg"
						style="width: 150rpx;height: 150rpx; border-radius: 50%;"
					></image>
				</view>
				<view>{{ user.username }}</view>
				<uni-list style="width: 750rpx;">
					<uni-list-item
						v-for="item in menu"
						:key="item.id"
						:show-extra-icon="true"
						showArrow
						clickable
						:extra-icon="item.icon"
						:title="item.title"
						@click="changeMenu(item.path)"
					/>
				</uni-list>
				<!-- 购物车 -->
		
				<!--12321312321  -->
				<view class="btn" hover-class="active" @click="logout" >
					切换账号
				</view>
				<view class="btn1" hover-class="active" @click="logout" >
					退出登录
				</view>
			</view>
	<!--  -->
<view class="con" v-if="!user">
	<view class="co">
	
	
		<view style="font-size: 32px;
		color: darkgrey;
		 font-weight: bold; text-align: center; font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;"
		 >别叫醒我</view>
		<!-- 登录表单 -->
		<view class="form">
			
				<uni-icons type="person" size="20" style="margin-right: 5px;"></uni-icons> 
				<input type="text" v-model="username"
				 placeholder="请输入账号" />
			</view>
			<view style="margin-top: 30rpx; display: flex; margin-left: 37%; margin-top: 20px;">
				<uni-icons type="locked" size="20" style="margin-right: 5px;"></uni-icons>
				<input type="password"
				v-model="password"
				  placeholder="请输入密码" />
			</view>
			<!--  -->
			
			  <view class="qqq">
			  	
			  
			  <uni-icons
			  type="arrow-right" size="30" color="white" @click="submit" hover-class="active">
				  
			  </uni-icons>
			  
			  </view>
			  <!--  -->
		</view>
		
		
		<!-- 其他登录分割 -->
		<view class="other" style="margin-left: 139px; margin-bottom: 15px;">其他方式登录</view>
					<!-- 其他登录图标 -->
					<view>
						<uni-icons
							type="qq"
							color=" "
							size="30"
							style= " background-color: transparent;width: 100rpx;height: 100rpx;border-radius: 50%; margin-left: 137px;
							;"
						></uni-icons>
						<uni-icons
							type="weixin"
							color="breen "
							size="30"
							style="background-color: transparent;width: 100rpx;height: 100rpx;border-radius: 50%; margin-left: 3px;
							;"
						></uni-icons>
						<uni-icons
							type="weibo"
							color="sorrel"
							size="30"
							style="background-color:transparent;width: 100rpx;height: 100rpx;border-radius: 50%; margin-left: 3px;
							;"
						></uni-icons>
					</view>
					
		<!--  -->
		</view>
	</view>
	</view>
</template>

<script>
export default {
	data() {
		return {        
			          extraIcon1:{type:'gear-filled'}, 
				        username: '',
						password: '',
						user: uni.getStorageSync('user'),
						menu: [
										{
											id: 1,
											title: '我的收货地址',
											icon: {
												color: 'darkgrey',
												size: '22',
												type: 'shop'
											},
											path: '/pages/ucenter/address'
										},
										{
											id: 2,
											title: '我的钱包',
											icon: {
												color: 'darkgrey',
												size: '22',
												type: 'wallet'
											},
											path: '/pages/address/address'
										},
										{
											id: 3,
											title: '我的订单',
											icon: {
												color: 'darkgrey',
												size: '22',
												type: 'gift-filled'
											},
											path: '/pages/address/address'
										},
										{
											id: 4,
											title: '我的黑名单',
											icon: {
												color: 'darkgrey',
												size: '22',
												type: 'eye-slash-filled'
											},
											path: '/pages/address/address'
										},
										{
											id: 5,
											title: '我的收藏',
											icon: {
												color: 'darkgrey',
												size: '22',
												type: 'star-filled'
											},
											path: '/pages/address/address'
										},
										{
											id: 6,
											title: '设置',
											icon: {
												color: 'red',
												size: '22',
												type: 'gear-filled'
											},
											path: '/pages/setting/setting'
										}],
									
										
										
									
		}
	
	
	
	} ,
	methods: {
		submit(){
			let data = {
				username: this.username,
				password: this.password
			}
			uni.request({
							url: 'http://127.0.0.1:5000/api/v1/login',
							method: 'POST',
							data,
							success(res) {
								console.log(res)
								if (res.data.code === 0) {
									uni.showToast({
										title: '登录成功'
									})
									// 存储本地
									uni.setStorageSync('token', res.data.token)
									uni.setStorageSync('user', res.data.data)
									window.location.reload()
								} else {
									uni.showToast({
										title: '登录失败',
										icon: 'error'
									})
								}
							}
						})
						},
							changeMenu(url) {
								    
									console.log('点击菜单了')
									console.log(url)
									uni.navigateTo({
										url
									})
								},
									logout() {
											uni.removeStorageSync('token')
											uni.removeStorageSync('user')
											window.location.reload()
										},
									
												// path: '/pages/address/address'
												  buttonClick (e) {
													    console.log(e)
													    this.options[2].info++
													  }
						
			
			
		
	}
}
</script>

<style lang="scss">
	.active {
		opacity: 0.5;
	}
	.con{background-image: url("https://wx3.sinaimg.cn/orj360/006UCet1ly1h7u72zo8h1j30u01t0451.jpg");
	height: 92vh;
	  background-size: 100%,100%;
	  display: flex;
	  flex-direction: column;
	  justify-content: center;
	  overflow: hidden;
	  }
	  .co{margin-bottom: 200px;}
	  .form {margin-left: 37%; margin-top: 20px;
	  		padding-top: 45rpx;
			
			display: flex;
	  		
	  	}
		.qqq{width: 70px;height: 70px;background-color: darkgray;border-radius: 50px; text-align: center;
		line-height: 70px; margin-left:152px; margin-top: 50px;}
		.btn{font-weight: bold; color: #ccc; width: 100px; height: 20px; border: 3px solid #ccc;text-align: center;
		border-radius: 50px; line-height: 20px; margin-top: 35px;}
		.btn1{font-weight: bold; color: crimson; width: 100px; height: 20px; border: 3px solid #ccc;text-align: center;
		border-radius: 50px; line-height: 20px; margin-top: 7px;}
		/*  */
		.example-body {
			padding: 0;
			/* #ifndef APP-NVUE */
			display: block;
			/* #endif */
		}
		
		.goods-carts {
			/* #ifndef APP-NVUE */
			display: flex;
			/* #endif */
			flex-direction: column;
			position: fixed;
			left: 0;
			right: 0;
			/* #ifdef H5 */
			left: var(--window-left);
			right: var(--window-right);
			/* #endif */
			bottom: 0;
		}
		.uni-container{width: 750rpx;}
		
		/*  */
</style>
