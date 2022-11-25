<template>
	<view class="cate-list">
		<!-- 左边分类 -->
		<scroll-view scroll-y="true" class="left-cate-list">
			<view @click="cateHandle(index)" :class="['cate-item', isActive == index ? 'active' : '']" v-for="(item, index) in cateList" :key="index">
				{{item.cat_name}}
			</view>
		</scroll-view>
		
		<!-- 右边分类 -->
		<view class="right-cate-children-list">
			<view class="children-item" v-for="item1 in cateList[isActive].children" :key="item1.cat_id">
				<view class="goods-item">
					<image src="../../static/small.png" mode="widthFix"></image>
					<text>{{item1.cat_name}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cateList: [],
				isActive: 0
			};
		},
		methods: {
			async getCateList(){
				const {data: res} = await uni.$http.get('/api/public/v1/categories')
				if(res.meta.status != 200){
					return uni.showToast({
						title: "加载超时",
						duration: 1500
					})
				}				
				this.cateList = res.message
				console.log(this.cateList)
			},
			cateHandle(index){
				this.isActive = index
				console.log("isActive:"+ this.isActive);
			}
		},
		onLoad() {
			this.getCateList()
		}
	}
</script>

<style lang="scss">
	.cate-list{	
		display: flex;
		
		.left-cate-list{
			display: flex;
			justify-content: space-between;
			flex-direction: column;
			width: fit-content;
			
			.cate-item{
				width: 150rpx;
				height: 60rpx;
				text-align: center;
				font-size: 14px;
				line-height: 60rpx;
				color: $uni-color-title;
				
				// 激活项的样式
				&.active {
					background-color: #FFFFFF;
					position: relative;
					background-color: #c00000;
					color: #FFFFFF; 
					&::before {
						content: '';
						display: block;
						width: 3px;
						height: 30px;
						background-color: #c00000;
						position: absolute;
						left: 0;
						top: 50%;
						transform: translateY(-50%);
					}
				}
			}
		}
		.right-cate-children-list{
			display: flex;
			flex-wrap: wrap;
			
			.children-item{
				width: 33.33%;
				margin-bottom: 10px;
				display: flex;
				flex-direction: column;
				align-items: center;
				font-size: 12px;
						
				image {
					width: 60px;
					height: 60px;
				}
			}
		}
	}
</style>
