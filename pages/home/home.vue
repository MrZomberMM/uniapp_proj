<template>
	<view>
		<!-- 轮播图 -->
		<swiper :indicator-dots="true" circular="true" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item v-for="(item, i) in swiperlist" :key="i">
				<navigator class="swiper-item" :url="item.navigator_url">
					<image :src="item.image_src" mode=""></image>
				</navigator>
			</swiper-item>
		</swiper>
		
		<!-- 首页分类选项 -->
		<view class="catitems-list" >
			<navigator url="/cate/cate.vue" open-type="switchTab" class="catitems-img" v-for="(item, i) in catitemList" :key="i">
				<image :src="item.image_src" mode=""></image>
			</navigator>
		</view>
		
		<!-- 瀑布流 -->
		<view class="floor">
			<view class="floor_list" v-for="(item, i) in floordataList" :key="i">
				<view class="floor-title" >
					<image :src="item.floor_title.image_src" mode="widthFix"></image>
				</view>
				<view class="floor-img-box">
					<image :src="item.product_list[0].image_src" mode="widthFix" :style="[{width: item.product_list[0].image_width + 'px'}]"></image>
					<view class="right-img-box">
						<block v-for="(item2, i2) in item.product_list" :key="i2">
							<image v-if="i2 !== 0" :src="item2.image_src" mode="widthFix" :style="[{width: item2.image_width + 'rpx'}]"></image>
						</block>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperlist: [],
				catitemList: [],
				floordataList: []
			};
		},
		methods:{
			async getSwiperList(){
				const {data: res} = await uni.$http.get('/api/public/v1/home/swiperdata')
				if(res.meta.status != 200){
					return uni.showToast({
						title: "加载超时",
						duration: 1500
					})
				}
				this.swiperlist = res.message
			},
			async getCatitems(){
				const {data: res} = await uni.$http.get('/api/public/v1/home/catitems')
				if(res.meta.status != 200){
					return uni.showToast({
						title: "加载超时",
						duration: 1500
					})
				}
				this.catitemList = res.message
			},
			async getFloordataList(){
				const {data: res} = await uni.$http.get('/api/public/v1/home/floordata')
				if(res.meta.status != 200){
					return uni.showToast({
						title: "加载超时",
						duration: 1500
					})
				}
				this.floordataList = res.message
				console.log(this.floordataList);
			}
		},
		onLoad(){
			this.getSwiperList()
			this.getCatitems()
			this.getFloordataList()
		}
	}
</script>

<style lang="scss">
	swiper{
		height: 330 rpx;
		
		.swiper-item,
		image{
			height: 100%;
			width: 100%;
		}
	}
	.catitems-list{
		display: flex;
		justify-content: space-around;
		margin: 15px 0;

		.catitems-img{
			width: 128rpx;
			height: 140rpx;
			
			image{
				margin:  0 10px;
				width: 100rpx;
				height: 100rpx;
			}
		}
	}
	.floor_list{
		.floor-title {
			height: 60rpx;
			width: 100%;
			display: flex;
		}
	
		.floor-img-box {
			display: flex;
			padding-left: 10rpx;
			
			.right-img-box {
				display: flex;
				flex-wrap: wrap;
				justify-content: space-around;
			}
		}
	}
</style>
