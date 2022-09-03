<template>
	<view class="imgListContainsr">
		<!-- 下拉框 -->
		<view class="downBox">
			<uni-data-select
			      v-model="value"
			      :localdata="range"
			      @change="changeList"
				  :clear="false"
			    ></uni-data-select>
		</view>
			<!-- 图片轮播 -->
		<view class="imgBox">
			<uni-swiper-dot class="imgList" :info="imgList" :current="current" field="content" :mode="mode">
				<swiper v-if="showSwiper" class="swiper-box" @change="change">
					<swiper-item class="swiperStyle" v-for="(item ,index) in imgList" :key="index">
						<view class="swiper-item">
							<image :src="item"></image>
							<view class="imgTitle">{{ imgTitle }}</view>
						</view>
					</swiper-item>
				</swiper>
			</uni-swiper-dot>
		</view>
		<!-- 切换按钮 -->
		<button class="btn" @click="taggerImg">切换图片</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 图片列表
				imgList: [],
				// 图片介绍
				imgTitle: '',
				current: 0,
				mode: 'default',
				value: 0,
				showSwiper: true,
				range: [
				  { value: 0, text: "女仆" },
				  { value: 1, text: "兔女郎" },
				  { value: 2, text: "夏日泳装" },
				  { value: 3, text: "动漫类" },
				  { value: 4, text: "萝莉" },
				  { value: 5, text: "少女" },
				  { value: 6, text: "御姐" },
				  { value: 7, text: "黑丝" },
				  { value: 8, text: "白丝" },
				  { value: 9, text: "肉丝" },
				  { value: 10, text: "腿控" },
				  { value: 11, text: "脚控" },
				  { value: 12, text: "旗袍" }
				],
			}
		},
		methods: {
			taggerImg() {
				console.log(this.range[this.value].text)
				this.showSwiper = false
				wx.request({
					method:"GET",
					url: 'https://api.uomg.com/api/rand.img4',
					data: {
						sort: this.range[this.value].text,
						format: 'json'
					},
					success:(res) => {
						console.log(res)
						if(res.data.code === 1) {
							this.imgList = []
							this.imgList = res.data.imgurl
							this.imgTitle = res.data.title
							this.showSwiper = true
						} else {
							wx.showToast({
								title:'数据请求失败'
							})
						}
					}
				})
			},
			change(e) {
				this.current = e.detail.current;
			},
			changeList(e) {
				this.value = e
			    this.taggerImg()
			},
		},
		created() {
			this.taggerImg()
		}
	}
</script>

<style lang="less" scoped>
	.imgListContainsr{
		height: 100vh;
		width: 750rpx;
		display: flex;
		flex-direction: column;
		.downBox{
			height: 100rpx;
			// background-color: red;
			margin-bottom: 10rpx;
		}
		/deep/.uni-select{
			height: 100rpx;
			line-height: 100rpx;
			font-size: 46rpx;
			padding-left: 25rpx;
		}
		/deep/.uni-select__selector-item{
			font-size: 42rpx;
		}
		/deep/uni-swiper-dot{
			flex: 1;
		}
		.imgBox{
			flex: 1;
			// height: 900rpx;
			display: flex;
			flex-direction: column;
			width: 750rpx;
			// height: 900rpx;
			.swiper-box{
				height: 80vh;
				.swiperStyle{
					height: 100%;
				}
			}
			.swiper-item{
				// flex: 1;
				position: relative;
				// height: 900rpx;
				// width: 750rpx;
				height: 100%;
				> image {
					height: 100%;
					width: 100%;
				}
				.imgTitle{
					position: absolute;
					bottom: 0;
					left: 0;
					width: 100%;
					height: 90rpx;
					background-color: rgba(0,0,0,0.3);
					color: white;
					line-height: 90rpx;
					font-size: 32rpx;
					padding-left: 25rpx;
				}
			}
		}
		.imgList {
			// height: 900rpx;
			flex: 1;
			width: 750rpx;
		}
		.btn{
			position: fixed;
			bottom: 30rpx;
			left: 50%;
			transform: translateX(-50%);
			height: 100rpx;
			line-height: 100rpx;
			color: white;
			width: 700rpx;
			background-color: #007AFF;
		}
	}
</style>
