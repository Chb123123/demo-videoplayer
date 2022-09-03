<template>
	<view class="CartoonContainer">
		<uni-popup ref="popup" type="dialog">
			<uni-popup-dialog message="下拉刷新" :duration="2000" :before-close="true" @close="close" @confirm="confirm">图片通过下拉的方式刷新</uni-popup-dialog>
		</uni-popup>
		<image class="imgStyle" :src="CartoonItem"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				CartoonItem: {}
			}
		},
		onPullDownRefresh() {
			this.getCartoonList()
		},
		methods: {
			getCartoonList() {
				wx.request({
					method:"GET",
					url:'https://www.dmoe.cc/random.php',
					data:{
						return: 'json'
					},
					success: (res) => {
						console.log(res.data)
						if(res.data.code === "200") {
							console.log(res.data)
							this.CartoonItem = res.data.imgurl
						} else {
							wx.showToast({
								title: '数据获取失败'
							})
						}
						wx.stopPullDownRefresh()
					}
				})
			},
			open() {
						this.$refs.popup.open()
			},
			close() {
						// TODO 做一些其他的事情，before-close 为true的情况下，手动执行 close 才会关闭对话框
						this.$refs.popup.close()
					},
			confirm(value) {
						// 输入框的值
						console.log(value)
						// TODO 做一些其他的事情，手动执行 close 才会关闭对话框
						this.$refs.popup.close()
					}
		},
		created() {
			this.getCartoonList()
		},
		mounted() {
			this.open()
		}
	}
</script>

<style lang="less" scoped>
	.CartoonContainer{
		height: 100vh;
		width: 750rpx;
		.imgStyle{
			width: 750rpx;
			height: 100vh;
		}
	}
</style>
