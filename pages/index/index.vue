<template>
	<view class="content">
		<view class="carList">
			<ul class="warp-car-ul" >
				<li class="warp-car-ul-li" v-for="(item,index) in carListDate" :key="item.index" @click="carListClick(index)">
					<div class="car-img-wrap">
						<image class="car-img" :src="item.imageurl" v-if="item.imageurl" @error="setErrorImg" mode="aspectFit"></image>
						<image class="car-img" :src="errSrc" v-else ></image>
					</div>
					<div class="des-model-wrap">
						<span class="warp-car-ul-span">{{item.cartype}}</span>
					</div>
				</li>
			</ul>
		</view>
	</view>
</template>

<script>
	import { carList } from '../../static/const/api.js'
	export default {
		data() {
			return {
				scrollTop:0,
				statu:'aaa',
				carListDate:[],
				errSrc:require('../../static/img/noface.gif'),
			}
		},
		onLoad() {
			this.getTestCars()
		},
		methods: {
			//选取试驾车
			carListClick (index,item) {
				console.log(this.carListDate[index])
			},
			//试驾车照片容错处理
			setErrorImg(e){
				console.error('image发生error事件，携带值为' + e.detail.errMsg)
			},
			getTestCars(){
				uni.request({
					url: carList, //仅为示例，并非真实接口地址。
					method :'POST',
					data: {
						"brandid":"d1a96b07e54f4cd1a05306629a6a530f",
						"tenantId":"08d5f0c68cc94b94af8c9d1dee768cb9"
					},
					header: {
							'content-type': 'application/json' //自定义请求头信息
					},
					success: (res) => {
							console.log(res.data);
							this.carListDate = res.data.data;//请求接口的数据
					}
				});
			}
		}
	}
</script>

<style lang="scss">
	.content {
		.scroll-Y{
			height: 200upx;
		}
		.scroll-view-item{
			height: 300upx;
		}
		.carList{
			.warp-car-ul {
				overflow: hidden;
				padding-bottom: 60upx;
				display: flex;
				flex-wrap:wrap;
				margin: 0 24upx;
				justify-content:space-between;
			}
			.warp-car-ul-li {
				width: 345upx;
				float: left;
				text-align: center;
				padding-bottom: 20upx;
				margin-bottom: 10upx;
				background-color: #fff;
				border-radius: 10upx;
				box-sizing: border-box;
				border: 3upx solid transparent;
				overflow: hidden;
			}
			.warp-car-ul-li:nth-child(2n+1) {
				border: 3upx solid transparent;
				box-sizing: border-box;
				margin-right: 6upx;
			}
			.warp-car-ul-li:nth-child(2n+2) {
				border: 3upx solid transparent;
				box-sizing: border-box;
			}
			.warp-car-ul-li:active {
				border: 3upx solid #61A5FF;
			}
			.warp-car-ul-li.active {
				border: 3upx solid #61A5FF;
			}
			.car-img-wrap{
				width: 100%;
				height: 345upx;
				display: flex;
				align-items: center;
				overflow: hidden;
				box-sizing: border-box;
				background-size:100% ;
			}
			.car-img {
				width: 100%;
				pointer-events: none;
				vertical-align: sub;
				align-items: center;
			}
			.des-model-wrap {
				display: table-cell;
				height: 90upx;
				vertical-align: middle;
				text-align: left;
				color: #444d66;
				width: 100%;
				overflow: hidden;
			}
			.warp-car-ul-span {
				width: 100%;
				line-height: 1.7 !important;
				box-sizing: border-box !important;
				display: -webkit-box !important;
				word-break: break-all !important;
				-webkit-box-orient: vertical !important;
				text-overflow: ellipsis !important;
				-webkit-line-clamp: 2 !important;
				word-wrap: break-word !important;
				padding: 0 10upx;
				font-size: 26upx;
				overflow: hidden !important;
			}
		}
	}
		


</style>
