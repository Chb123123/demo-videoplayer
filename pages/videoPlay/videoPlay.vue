<template>
	<view class="palyVideoContainer">
		<!-- <view class="videoTitle">标题</view> -->
		<view class="videoBox">
			<video :src="videoMp4Url" autoplay play-btn-position="center" class="videoStyle"></video>
		</view>
		<view class="saying">
			<view class="everySaying">每日名言</view>
			<view>{{ saying.name }}</view>
			<view class="sayingAuthor">--- {{ saying.from }}</view>
		</view>
	</view>
</template>

<script>
	export default{
		data() {
			return {
				videoMp4Url: "",
				saying: {}
			}
		},
		onLoad: function(option) {
			this.videoMp4Url = option.videoPlay
			// console.log(option.videoPlay)
			this.getEverySering()
		},
		methods: {
			async getEverySering() {
				wx.request({
					method: "get",
					url: "https://api.apiopen.top/api/sentences",
					success: (res) => {
						if(res.data.code === 200) {
							console.log(res.data)
							this.saying = res.data.result
						}
					}
				})
				// const res = await this.$axios({
				// 	url: "sentences"
				// 	// method: "get"
				// })
				// console.log(res.data.result.name)
				// this.saying = res.data.result
			}
		}
	}
</script>

<style lang="less" scoped>
	.palyVideoContainer{
		// background-color: red;
	}
	.videoTitle{
		height: 100rpx;
		line-height: 100rpx;
		text-align: center;
		font-size: 32rpx;
	}
	.videoBox{
		height: 500rpx;
		width: 750rpx;
		// background-color: red;
	}
	.videoStyle{
		width: 750rpx;
		height: 500rpx;
	}
	.saying{
		position: absolute;
		display: flex;
		flex-direction: column;
		bottom: 100rpx;
		// left: 50rpx;
		// background-color: red;
		padding: 30rpx;
		width: 750rpx;
	}
	.everySaying{
		text-align: center;
		margin-bottom: 20px;
		text-overflow: ellipsis;
		white-space: nowrap;
		overflow: hidden;
	}
	.sayingAuthor{
		position: absolute;
		// background-color: blue;
		right: 0;
		bottom: -60rpx;
		padding-left: 100rpx;
	}
</style>
