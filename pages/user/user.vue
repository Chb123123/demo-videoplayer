<template>
	<view class="userContainer">
		<view class="head">
			<view class="headLeft">
				<uni-icons :type="collectStyle.state" size="70rpx" :color="collectStyle.color" style="margin-bottom: 10rpx" @click="collectBtn"></uni-icons>
				收藏
			</view>
			<view class="avatar">
				<image v-if="localImg" class="avatarStyle" :src="localImg" mode="aspectFill" @click="taggerImg"></image>
				<view v-else class="uploadimg" @click="taggerImg">上传图片</view>
				<text @click="gotoLogin">点击登录</text>
			</view>
			<view class="headRight">
				<uni-icons type="hand-up" size="70rpx" :color="likecolor" style="margin-bottom: 10rpx" @click="like"></uni-icons>
				点赞
			</view>
		</view>
		<view class="NavList">
			<view v-for="(item, index) in navBgList" :key="index" :style="'background-color:' + item.navBg" class="NvaStyle">{{ item.navTitle }}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 收藏的状态
				collectStyle: {
					color: "#fff",
					state: "star"
				},
				// 点赞的颜色
				likecolor: "#fff",
				// 上传图片的地址
				localImg: null,
				navBgList: [
					{ navTitle: '我的商城', navBg: "#FDB91A" },
					{ navTitle: '我的微博', navBg: "#F48081" },
					{ navTitle: '品牌简介', navBg: "#A0D350" },
					{ navTitle: '微名片', navBg: "#A0D350" },
					{ navTitle: '我的相册', navBg: "#86D5F6" },
					{ navTitle: '新闻资讯', navBg: "#C99CD7" },
					{ navTitle: '给我留言', navBg: "#3ACBC6" },
					{ navTitle: '关注更多', navBg: '#BFBFBF' }
				]
			}
		},
		methods: {
			gotoLogin() {
				console.log('11')
			},
			taggerImg() {
				uni.chooseImage({
					count: 1,
					success:(res) => {
						this.localImg = res.tempFilePaths[0]
						console.log(res.tempFilePaths[0])
					}
				})
			},
			like() {
				if(this.likecolor === "#fff") {
					this.likecolor = "#f00"
				} else{
					this.likecolor = "#fff"
				}
			},
			collectBtn() {
				if(this.collectStyle.color === "#fff") {
					this.collectStyle.color = "#ff0",
					this.collectStyle.state = "star-filled"
				} else {
					this.collectStyle.color = "#fff",
					this.collectStyle.state = "star"
				}
			},
			getVerificationCode() {
				uni.request({
					method: "post",
					url: "https://api.apiopen.top/searchMusic",
					data: {
						name: "好久不见"
					},
					success: (res) => {
						console.log(res)
					}
				})
			}
		},
		created() {
			this.getVerificationCode()
		}
	}
</script>

<style lang="less" scoped>
	.userContainer{
		height: 100vh;
		width: 750rpx;
		background-color: #eee;
		.head{
			height: 360rpx;
			width: 750rpx;
			background-color: #363B41;
			color: #fff;
			padding: 90rpx 0 0;
			display: flex;
			margin-bottom: 30rpx;
			.headLeft{
				flex: 1;
				text-align: center;
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
			}
			.avatar{
				width: 155rpx;
				height: 215rpx;
				// background-color: red;
				text-align: center;
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				.avatarStyle{
					width: 155rpx;
					height: 155rpx;
					border-radius: 50%;
				}
				.uploadimg{
					width: 155rpx;
					height: 155rpx;
					border-radius: 50%;
					line-height: 155rpx;
					color: #666;
					background-color: #FAFAFA;
					text-align: center;
					font-size: 32rpx;
				}
			}
			.headRight{
				flex: 1;
				text-align: center;
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				color: #fff;
				font-size: 34rpx;
			}
		}
		.NavList{
			padding: 30rpx 45rpx;
			background-color: #fff;
			display: flex;
			flex-wrap: wrap;
			justify-content: space-between;
			.NvaStyle{
				width: 320rpx;
				height: 120rpx;
				margin-bottom: 15rpx;
				border-radius: 30rpx;
				background-color: red;
				line-height: 120rpx;
				color: #fff;
				text-align: center;
				font-size: 45rpx;
			}
		}
	}
</style>
