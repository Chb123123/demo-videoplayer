<template>
	<view class="videoContainer" @click="getVideoMp4">
		<image class="videoStyle" :src="videoImg" mode="aspectFit"></image>
		<text class="videoTitle">{{ videoTitle }}</text>
	</view>
</template>

<script>
	export default{
		props: {
			// 视频图片
			videoImg: {},
			// 视频介绍
			videoTitle: {},
			// 视频地址
			videoMp4: {}
		},
		methods: {
			getVideoMp4: function() {
				uni.navigateTo({
					url: "/pages/videoPlay/videoPlay?videoPlay=" + this.videoMp4
				})
				// console.log(this.videoMp4)
			}
		}
	}
</script>

<style lang="less" scoped>
	.videoContainer{
		display: flex;
		flex-direction: column;
		// justify-content: space-between;
		align-items: center;
		width: 335rpx;
		height: 400rpx;
		background-color: #fff;
		// border: 1rpx solid #ccc;
		// float: left;
		margin-bottom: 35rpx;
		padding: 0 25rpx 10rpx;
		border-radius: 30rpx;
	}
	.videoTitle{
		display: -webkit-box;
		-webkit-line-clamp: 2;
		overflow: hidden;
		text-overflow: ellipsis;
		height: 120rpx;
		line-height: 1.3;
		font-size: 30rpx;
		-webkit-box-orient: vertical;
		// text-overflow: ellipsis;
		// overflow: hidden;
		// white-space: nowrap;
		// background-color: red;
		// height: 30rpx;
	}
	.videoStyle{
		width: 100%;
	}
</style>
