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
			<uni-swiper-dot :info="info" :current="current" field="content" :mode="mode">
				<swiper class="swiper-box" @change="change">
					<swiper-item v-for="(item ,index) in imgList" :key="index">
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
				mode: 'round',
				value: 0,
				      range: [
				        { value: 0, text: "全部" },
				        { value: 1, text: "女仆" },
				        { value: 2, text: "JK制服" },
				        { value: 3, text: "兔女郎" },
				        { value: 4, text: "夏日泳装" },
				        { value: 5, text: "动漫类" },
				        { value: 6, text: "萝莉" },
				        { value: 7, text: "少女" },
				        { value: 8, text: "御姐" },
				        { value: 9, text: "黑丝" },
				        { value: 10, text: "白丝" },
				        { value: 11, text: "肉丝" },
				        { value: 12, text: "渔网" },
				        { value: 13, text: "吊带娃" },
				        { value: 14, text: "腿控" },
				        { value: 15, text: "脚控" },
				        { value: 16, text: "臀控" },
				        { value: 17, text: "旗袍" },
				      ],
			}
		},
		methods: {
			taggerImg() {
				wx.request({
					method:"GET",
					url: 'https://api.uomg.com/api/rand.img4',
					data: {
						sort: this.range[this.current].text,
						format: 'json'
					},
					success:(res) => {
						console.log(res)
						if(res.data.code === 1) {
							this.imgList = res.data.imgurl
							this.imgTitle = res.data.title
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
			    this.taggerImg()
			},
		}
	}
</script>

<style lang="less" scoped>
	.imgListContainsr{
		height: 100vh;
		width: 750rpx;
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
		.imgBox{
			height: 900rpx;
			.swiper-item{
				position: relative;
				height: 900rpx;
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
		/deep/swiper {
			height: 900rpx;
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
