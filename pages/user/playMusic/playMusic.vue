<template>
	<view class="playMusicContainer">
		<view class="musicImg">
			<image :src="musicBg || '../../../static/logo.png'" @load="showImg" mode="" :style="'animation-play-state:' + musicPlay + ';'"></image>
		</view>
		<view class="playBtnBox">
			<audio :src="musicUrl" loop="true" id="myAudio"></audio>
			<!-- <view class="musicState">
				<view class="like">
					<uni-icons type="heart" size="50rpx" color="white"></uni-icons>
				</view>
				<view class="like">
					<uni-icons type="hand-up" size="50rpx" color="white"></uni-icons>
				</view>
				<view class="like">
					<uni-icons type="list" size="50rpx" color="white"></uni-icons>
				</view>
				<view class="like">
					<uni-icons type="redo-filled" size="50rpx" color="white"></uni-icons>
				</view>
			</view> -->
			<view class="musicBtn">
				<view class="musicbtnStyle" @click="showState">
					<uni-icons type="refreshempty" size="50rpx" color="white"></uni-icons>
				</view>
				<view class="musicbtnStyle" @click="backMusic">
					<uni-icons type="back" size="50rpx" color="white"></uni-icons>
				</view>
				<view class="musicbtnStyle" @click="playBtn">
					<uni-icons type="headphones" size="95rpx" color="white"></uni-icons>
				</view>
				<view class="musicbtnStyle" @click="taggerMusic">
					<uni-icons type="forward" size="50rpx" color="white"></uni-icons>
				</view>
				<view class="musicbtnStyle" @click="showDrawer" >
					<uni-icons type="settings-filled" size="50rpx" color="white"></uni-icons>
				</view>
			</view>
		</view>
		<view>
			<uni-drawer ref="showRight" mode="right" maskClick :mask-click="false">
				<scroll-view style="height: 100%;width: 600rpx;background-color: #fff;" scroll-y="true">
					<view class="musicStyle" v-for="(item, index) in likeMusicList" :key="index" @click="toggleMusic(item)">
						<view class="musicName">{{ item.name }}</view>
						<view class="musicAudio">{{ item.author }}</view>
					</view>
				</scroll-view>
			</uni-drawer>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 音乐封面
				musicBg: null,
				// 音乐名称
				musicName: {},
				// 音乐地址
				musicUrl: null,
				// 音乐作者
				musicAuthor: {},
				// 音乐是否请求完成
				flag: true,
				// 将收藏的音乐存放在列表
				likeMusicList: [],
				// 音乐是否播放
				isPlayMusic: false,
				// 音乐播放，图片旋转
				musicPlay: 'paused',
				// 当前音乐回退的数量
				backMusicCount: 0
			}
		},
		onReady: function (e) {
		    // 使用 wx.createAudioContext 获取 audio 上下文 context
		    this.audioCtx = wx.createAudioContext('myAudio')
		    this.musicBg = wx.createAudioContext('myMusic')
		  },
		methods: {
			showDrawer() {
				this.$refs.showRight.open();
			},
			playBtn() {
				if(this.isPlayMusic) {
					console.log('点击暂停按钮')
					this.isPlayMusic = false
					this.audioCtx.pause()
					this.musicPlay = 'paused'
				} else {
					this.isPlayMusic = true
					console.log('点击播放按钮')
					this.audioCtx.play()
					this.musicPlay = 'running'
				}
				// wx.createSelectorQuery().select("#myAudio").play()
			},
			showState() {
				wx.showToast({
					title: '当前只支持随机模式'
				})
			},
			toggleMusic(obj) {
				this.musicBg = obj.img
				this.musicUrl = obj.url
				this.musicName = obj.name
				this.musicAuthor = obj.author
				this.musicPlay = 'paused'
				this.$refs.showRight.close()
			},
			// 返回上一首音乐
			backMusic() {
				if(this.likeMusicList.length <= 1) {
					wx.showToast({
						title: '没有更多歌曲'
					})
				} else {
					this.backMusicCount += 1
					if(this.likeMusicList.length > this.backMusicCount) {
						this.musicBg = this.likeMusicList[this.backMusicCount].img
						this.musicUrl = this.likeMusicList[this.backMusicCount].url
						this.musicName = this.likeMusicList[this.backMusicCount].name
						this.musicAuthor = this.likeMusicList[this.backMusicCount].author
						this.musicPlay = 'paused'
						this.isPlayMusic = false
					} else {
						wx.showToast({
							title: '没有更多歌曲'
						})
					}
				}
			},
			// 切换歌曲
			taggerMusic() {
				this.backMusicCount = 0
				console.log(this.likeMusicList)
				if(this.flag) {
					this.isPlayMusic = false
					this.flag = false
					console.log('点击了切换音乐')
					this.getMusic()
					this.musicPlay = 'paused'
					// this.audioCtx.play()
				} else {
					wx.showToast({
						title:'切换歌曲过快'
					})
				}
			},
			showImg() {
				this.flag = true
			},
			getMusic() {
				wx.request({
					url: "https://api.uomg.com/api/rand.music",
					data:{
						sort: '热歌榜',
						format: 'json'
					},
					success: (res) => {
						// console.log(res.data.data)
						if(res.data.code === 1) {
							this.musicBg = res.data.data.picurl
							this.musicUrl = res.data.data.url
							this.musicName = res.data.data.name
							this.musicAuthor = res.data.data.artistsname
							// 将获取的音乐存放到likeMusicList
							this.likeMusicList.unshift({
								name: this.musicName,
								img: this.musicBg,
								url: this.musicUrl,
								author: this.musicAuthor
							})
						}
					}
				})
			}
		},
		created() {
			this.getMusic()
		}
	}
</script>

<style lang="less" scoped>
	.playMusicContainer{
		position: relative;
		height: 100vh;
		background: -webkit-linear-gradient(top, #08070C, #58381F, #140A09);
		.musicImg{
			display: flex;
			justify-content: center;
			align-items: center;
			width: 570rpx;
			height: 570rpx;
			border-radius: 50%;
			overflow: hidden;
			position: absolute;
			top: 130rpx;
			left: 50%;
			transform: translateX(-50%);
			background-color: black;
			box-shadow: 0 0 30rpx rgba(255,255,255, 0.5);
			> image {
				height: 450rpx;
				width: 450rpx;
				border-radius: 50%;
				animation: move 5s infinite linear;
			}
		}
		.playBtnBox{
			display: flex;
			flex-direction: column;
			align-items: center;
			height: 290rpx;
			width: 750rpx;
			position: absolute;
			bottom: 0;
			left: 50%;
			transform: translateX(-50%);
			// border: 1rpx solid red;
			.musicState{
				display: flex;
				justify-content: space-between;
				width: 650rpx;
				height: 110rpx;
				// background-color: red;
				.like{
					flex: 1;
					height: 110rpx;
					line-height: 110rpx;
					text-align: center;
					// background-color: red;
				}
			}
			.musicBtn{
				height: 290rpx;
				width: 650rpx;
				// background-color: white;
				display: flex;
				justify-content: space-between;
				align-items: center;
				color: white;
				.musicbtnStyle{
					flex: 1;
					height: 180rpx;
					line-height: 180rpx;
					// background-color: red;
					text-align: center;
				}
			}
		}
		@keyframes move{
			0%{
				transform: rotateZ(0deg);
			}
			100% {
				transform: rotateZ(360deg);
			}
		}
		.musicStyle{
			display: flex;
			flex-direction: column;
			height: 120rpx;
			margin: 10rpx 0;
			padding-left: 25rpx;
			border-bottom: 1rpx solid #ccc;
			.musicName{
				height: 60rpx;
				font-size: 40rpx;
				line-height: 60rpx;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
			}
			.musicAudio{
				height: 40rpx;
				line-height: 40rpx;
				font-size: 32rpx;
				color: #777;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
			}
			.showRightTop{
				height: 100rpx;
				line-height: 100rpx;
				font-size: 54rpx;
				text-align: center;
				color: #303133;
			}
		}
	}
</style>
