<template>
	<view>
		<uni-search-bar @input="search" />
		<uni-list>
			<uni-list-item
				:title="item.name"
				:note="item.desc"
				v-for="item in indexList"
				:key="item.id"
			>
			<template v-slot:footer>
				<image :src="item.pic" style="width: 100rpx;height:100rpx"></image>
			</template>
			</uni-list-item>
		</uni-list>
	</view>
</template>

<script>
import url from '../../config/url'
export default {
	data() {
		return {
			indexList: []
		}
	},
	onLoad(option) {
		uni.request({
			url: url.search + option.keywords,
			success: res => {
				this.indexList = res.data
			}
		})
	},
	methods:{
		search(value){
			uni.request({
				url: url.search + value,
				success: res => {
					this.indexList = res.data
				}
			})
		}
	}
}
</script>

<style scoped></style>