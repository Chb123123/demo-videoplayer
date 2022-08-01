<template>
	<view class="content">
		<view class="navList">
			<view v-if="navNum === 0" class="navstyle checkedNav" @click="checkedBtn(0)">视频</view>
			<view v-else class="navstyle" @click="checkedBtn(0)">视频</view>
			<view v-if="navNum === 1" class="navstyle checkedNav" @click="checkedBtn(1)">图片</view>
			<view v-else class="navstyle" @click="checkedBtn(1)">图片</view>
		</view>
		<!-- <view class="title">视频</view> -->
		<view v-if="navNum === 0" class="videoBox">
			<VideoBox v-for="(item, index) in videoList.result.list" :key="index" :videoImg="item.userPic" :videoTitle="item.title" :videoMp4="item.playUrl"></VideoBox>
		</view>
		<view v-else class="imgBox">
			<view class="imgNavBox">
				<view v-for="(item, index) in imgNavList" :key="index" :class="item.style" @click="checkedNav(item.index, item.type)">{{ item.value }}</view>
			</view>
			<view class="imgStyleBox">
				<ImgList v-for="item in imgList.result.list" :key="item.id" :imgUrl="item.url"></ImgList>
			</view>
		</view>
	</view>
</template>

<script>
	// import { axios } from '../../util/defultUrl.js'
	import VideoBox from "../../components/videoBox.vue" 
	import ImgList from "../../components/imgStyle.vue"
	export default {
		components:{
			VideoBox,
			ImgList
		},
		data() {
			return {
				title: '首页',
				videoList: null,
				page: 0,
				// 显示的导航栏
				navNum: 0,
				// 当前显示的导航栏
				newNav: 0,
				// 请求图片的数量
				imgCount: 0,
				imgList: null,
				imgNavList: [
					{ type: '', index: 0, value: "全部", style: 'imgNav checkedImgNva' },
					{ type: 'animal', index: 1, value: "动物", style: 'imgNav' },
					{ type: 'beauty', index: 2, value: "美", style: 'imgNav' },
					{ type: 'car', index: 3, value: "汽车", style: 'imgNav' },
					{ type: 'comic', index: 4, value: "漫画", style: 'imgNav' },
					{ type: 'food', index: 5, value: "食物", style: 'imgNav' },
					{ type: 'game', index: 6, value: "游戏", style: 'imgNav' },
					{ type: 'movie', index: 7, value: "电影", style: 'imgNav' },
					{ type: 'person', index: 8, value: "人", style: 'imgNav' },
					{ type: 'phone', index: 9, value: "电话", style: 'imgNav' },
					{ type: 'scenery', index: 10, value: "风景", style: 'imgNav' }
				]
			}
		},
		onLoad() {
			this.getVideoList()
		},
		onPullDownRefresh() {
			this.downrenew()
		},
		onReachBottom() {
			if (this.navNum === 0) {
				this.page += 10
				this.getVideoList()
				console.log('11')
			}
			// console.log(this.getVideoList())
			// this.videoList.result.list = [...this.videoList.result.list]
		},
		methods: {
			getVideoList() {
				wx.request({
					method: 'get',
					url: "https://api.apiopen.top/api/getHaoKanVideo",
					data: { 
						page: this.page,
						size: 10
						},
						success:((res) => {
							// console.log(this.videoList)
							if(this.videoList) {
								this.videoList.result.list = [...this.videoList.result.list, ...res.data.result.list]
							} else{
								this.videoList = res.data
							}
							console.log(this.videoList)
						})
				})
				this.getImgList()
			},
			downrenew() {
				this.page += 10
				wx.request({
					method: 'get',
					url: "https://api.apiopen.top/api/getHaoKanVideo",
					data: { 
						page: this.page,
						size: 10
						},
						success:((res) => {
								this.videoList.result.list = [...res.data.result.list ,...this.videoList.result.list]
								wx.stopPullDownRefresh()
							// console.log(this.videoList)
						})
				})
			},
			checkedBtn(index) {
				// console.log(this)
				this.navNum = index
			},
			// 获取图片列表
			getImgList() {
				wx.request({
					method:"get",
					url:"https://api.apiopen.top/api/getImages",
					data:{
						page: this.imgCount,
						size: 10
					},
					success:(res) => {
						console.log(res.data)
						this.imgList = res.data
					}
				})
			},
			checkedNav(index, type) {
				// console.log(index, type)
				if(index !== this.newNav) {
					this.newNav = index
					this.imgCount = 0
					if(index !== 0) {
						wx.request({
							method: "get",
							url: "https://api.apiopen.top/api/getImages",
							data: {
								type: type,
								page: this.imgCount,
								size: 10
							},
							success:(res) => {
								this.imgList = res.data
							}
						})
					} else {
						wx.request({
							method: "get",
							url: "https://api.apiopen.top/api/getImages",
							data: {
								page: this.imgCount,
								size: 10
							},
							success:(res) => {
								this.imgList = res.data
							}
						})
					}
				}
				this.imgNavList.some((item) => {
					item.style = 'imgNav'
				})
				this.imgNavList[index].style = 'imgNav checkedImgNva'
			}
		},
		
		created() {
			this.getVideoList()
		}
	}
</script>

<style lang="less" scoped>
	.aa{
		background: url(https://f7.baidu.com/it/u=2369504500,1225809867&fm=222&app=108&f=JPEG@s_2,w_681,h_381,q_100);
	}
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	
	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	.navList{
		display: flex;
		width: 750rpx;
		height: 150rpx;
		// background-color: red;
		margin-bottom: 30rpx;
		.navstyle{
			flex: 1;
			background-color: #007AFF;
			display: flex;
			justify-content: center;
			align-items: center;
			color: white;
			font-size: 50rpx;
		}
		.checkedNav{
			background-color: #fff;
			color: #444;
		}
	}
	.videoBox{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		background-color: #eee;
		padding: 30rpx 30rpx 0;
		/* flex-direction: ; */
	}
	.title{
		height: 80rpx;
		line-height: 80rpx;
		text-align: center;
		font-size: 34rpx;
	}
	.imgNavBox{
		display: flex;
		flex-wrap: wrap;
		flex-direction: column;
		overflow-x: auto;
		// padding: 0rpx 30rpx;
		width: 750rpx;
		height: 100rpx;
		line-height: 100rpx;
		margin-bottom: 30rpx;
		// background-color: #ccc;
		.imgNav{
			float: left;
			height: 100rpx;
			margin-right: 40rpx;
			text-align: center;
			font-size: 40rpx;
			border-radius: 30rpx;
			background-color: #eee;
			color: #333;
			// border: 3rpx solid #ccc;
			padding: 0 30rpx;
		}
		.checkedImgNva{
			background-color: #007AFF;
			color: #fff;
			box-shadow: 0 0 10rpx rgba(0,0,0, 0.5);
		}
	}
	.imgStyleBox{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
</style>
