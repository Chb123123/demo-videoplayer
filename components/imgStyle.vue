<template>
	<view class="imgStyleContainer">
		<image v-if="timer" class="imgStyle" src="http://elm.cangdu.org/img/164ad0b6a3917599.jpg" @error="imageError(e)" mode="aspectFit"></image>
	</view>
</template>

<script>
	export default{
		props:{
			imgUrl: {
				default: null
			}
		},
		data() {
			return {
				timer: false
			}
		},
		methods:{
			imageError(e) {
				console.log('11')
			}
		},
		created() {
			setTimeout(() => {
				this.timer = true
			}, 2000)
			// console.log(this.imgUrl)
		}
	}
</script>

<style lang="less" scoped>
	.imgStyleContainer{
		margin-bottom: 30rpx;
		width: 330rpx;
		height: 300rpx;
		margin: 0 20rpx 20rpx;
		.imgStyle{
			width: 100%;
			height: 100%;
			border-radius: 50rpx;
		}
	}
</style>
