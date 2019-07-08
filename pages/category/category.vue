<template>
	<view class="content">
		<!-- header -->
		<view class="header"><input class="search-input" confirm-type="search" placeholder="请输入商品名称" /></view>
		<view class="category">
			<view class="nav-left">
				<scroll-view class="scollView" scroll-y >
					<view class="nav-left-item" @click="categoryClickMain(item,index)" :key="index" :class="index==categoryActive?'active':''" v-for="(item,index) in categoryList">
						{{item.NAME}}
					</view>
				</scroll-view>
			</view>
			<view class="nav-right">
				<scroll-view class="scollView"  scroll-y :scroll-top="scrollTop" @scroll="scroll"  scroll-with-animation >
					<view :id="index==0?'first':''" class="nav-right-item" v-for="item in subCategoryList" :key="item">
						<image class="rightImg" :src="item.LOGO" />
						<view>{{item.NAME}}</view>
					</view>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return{
				categoryList:[],
				subCategoryList: [],
				height:0,
				categoryActive:0,
				scrollTop:0,
				scrollHeight:0
			}
		},
		methods:{
			scroll(e){
				this.scrollHeight = e.detail.scrollHeight;
			},
			categoryClickMain(categroy, index) {
				this.categoryActive = index;
				this.subCategoryList = categroy.subCategoryList;
				this.scrollTop = -this.scrollHeight*index;
			},
			getCategory() {
				for(var i=0;i<20;i++){
					var subList = [];
					for(var j=0;j<30;j++){
						subList.push({"NAME":"分类"+i+":商品"+j,"LOGO":"http://placehold.it/50x50"})
					}
					this.categoryList.push({"NAME":"分类"+i,"subCategoryList":subList})
				}
				this.subCategoryList = this.categoryList[0].subCategoryList;
			}
		},
		onLoad:function(){
			this.getCategory()
			uni.getSystemInfo({
				success: res => {
					this.height = res.screenHeight;
				}
			})
		}
	}
</script>

<style lang="scss">
	.content{
		position: relative;
		.header{
			width:100%;
			box-sizing:border-box;
			padding: 0 20upx;
			display: flex;
			align-items:center;
			justify-content: space-between;
			font-size:13px; 
			height: 100upx;
			background: #ffffff;
			position: absolute;
			top: 0;
			.search-input{
				height: 62upx;
				width: 100%;
				border-radius:31upx; 
				background: #efefef;
				padding:0 20upx;
				box-sizing: border-box;
				flex:1;
			}
		}
		.category {
			padding-top: 100upx;
			border-top: solid 1upx #ddd;
			width: 100%;
			height: 100vh;
			box-sizing: border-box;
			overflow: hidden;
			display: flex;
			.nav-left {
				width: 30%;
				.nav-left-item {
					height: 100upx;
					border-right: solid 1upx #ddd;
					border-bottom: solid 1upx #ddd;
					font-size: 30upx;
					display: flex;
					align-items: center;
					justify-content: center;
				}
			}
			.nav-right {
				width: 70%;
				padding-top: 22upx;
				.nav-right-item {
					width: 28%;
					height: 220upx;
					float: left;
					text-align: center;
					padding: 11upx;
					font-size: 28upx;
					.rightImg{
						width: 100upx;
						height: 100upx;
					}
				}
			}
			.scollView{
				height: 100%;
			}
			.active {
				color: #F24544;
			}
		}

	}
</style>
