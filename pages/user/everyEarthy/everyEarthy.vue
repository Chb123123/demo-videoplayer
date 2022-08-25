<template>
	<!-- 土味情话 -->
	<view class="EarthyContainer">
		<view class="eartyhBox">{{ earthy }}</view>
		<button class="btn" @click="taggerEarthy">换 一 个</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 接收情话数据
				earthy: ''
			}
		},
		methods:{
			taggerEarthy() {
				wx.request({
					method:"GET",
					url: 'https://api.uomg.com/api/rand.qinghua',
					data: {
						format: 'json'
					},
					success:(res) => {
						console.log(res.data)
						if(res.data.code === 1) {
							this.earthy = res.data.content
						} else {
							wx.showToast({
								title:"数据请求失败!"
							})
						}
					}
				})
			}
		},
		onLoad() {
			this.taggerEarthy()
		}
	}
</script>

<style lang="less" scoped>
	.EarthyContainer{
		height: 100vh;
		width: 750rpx;
		padding-top: 50rpx;
		background-color: #87CEFF;
		.eartyhBox{
			height: 500rpx;
			width: 700rpx;
			background-color: white;
			margin: auto;
			border-radius: 30rpx;
			font-size: 45rpx;
			padding: 25rpx;
			line-height: 1.5;
			overflow: scroll;
		}
		.btn{
			width: 700rpx;
			height: 100rpx;
			margin: 100rpx auto 0;
			line-height: 100rpx;
			font-size: 60rpx;
			border-radius: 50rpx;
			background: -webkit-linear-gradient(left, Magenta, Orchid);
			box-shadow: 0 0 30rpx rgba(0,0,0,0.5);
			color: white;
		}
	}
</style>
