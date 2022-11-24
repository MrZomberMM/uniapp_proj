<template>
	<view>
		<swiper :indicator-dots="true" circular="true" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item v-for="item in swiperlist" :key="item">
				<navigator class="swiper-item" :url="item.navigator_url">
					<image :src="item.image_src" mode=""></image>
				</navigator>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperlist: []
			};
		},
		methods:{
			async getSwiperList(){
				const {data: res} = await uni.$http.get('/home/swiperdata')
				console.log(res);
				if(res.meta.status != 200){
					return uni.showToast({
						title: "加载超时",
						duration: 1500
					})
				}
				this.swiperlist = res.message
			}
		},
		onLoad(){
			this.getSwiperList()
		}
	}
</script>

<style lang="scss">
swiper{
	height: 240px;
	image{
		width: 100%;
	}
}
</style>
