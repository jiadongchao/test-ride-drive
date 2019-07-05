<template>
	<view class="content">
		<!-- header -->
		<view class="header"><span>{{city}}</span><input class="search-input" confirm-type="search" placeholder="请输入商品名称" /></view>
		<!-- banner -->
		<view class="swiper-wrap">
			<swiper class="swiperWrap" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular">
				<swiper-item  v-for="(item,index) in carListDate" :key="item.index" @click="carListClick(index)">
						<view class="swiper-item">
							<!-- <image class="car-img" :src="item.imageurl" v-if="item.imageurl" @error="setErrorImg" mode="aspectFit"></image -->
							<!-- <image class="car-img" :src="errSrc" v-else ></image> -->
							<image class="car-img" :src="carImg" v-if="item.imageurl" @error="aspectFit" mode="scaleToFill"></image>
						</view>
				</swiper-item>
			</swiper>
		</view>
		<!--中间菜单 -->
		<view class="menu-wrap">
			<ul class="menu-ul">
				<li class='menu-li' v-for="(item,index) in menus" :key="item.index" @click="menuClick(index)">
					<span>
						<image class="menu-icon" :src="item.icon" v-if="item.icon" mode="aspectFit"></image>
						<p >{{item.name}}</p>
					</span>
				</li>
			</ul>
		</view>
		<!-- goods -->
		<view class="goods-wrap">
			<view class="group">
				<view class="group-head">
					<image class="group-icon" src="../../static/img/svg/hot-car.svg"  mode="aspectFit"></image>
					<span class="group-title">热销车型</span>
					<span class="group-more">更多<span class="iconfont icon-you"></span></span>
				</view>
				<ul class="goods-ul">
					<li class="goods-li">
						<view class="li-content">
							<image class="goods-img" :src="carImg"  mode="aspectFit"></image>
							<view class="goods-type">奥迪19 A3 Sportback 1.4T 7档自动 进取型 国产-0513-6911-1</view>
							<view class="goods-price">优惠价<span>￥288，000.00</span></view>
						</view>
					</li>
					<li class="goods-li">
						<view class="li-content">
							<image class="goods-img" :src="carImg"  mode="aspectFit"></image>
							<view class="goods-type">奥迪19 A3 Sportback 1.4T 7档自动 进取型 国产-0513-6911-1</view>
							<view class="goods-price">优惠价<span>￥288，000.00</span></view>
						</view>
					</li>
					<li class="goods-li">
						<view class="li-content">
							<image class="goods-img" :src="carImg"  mode="aspectFit"></image>
							<view class="goods-type">奥迪19 A3 Sportback 1.4T 7档自动 进取型 国产-0513-6911-1</view>
							<view class="goods-price">优惠价<span>￥288，000.00</span></view>
						</view>
					</li>
				</ul>
				
			</view>
		</view>
		
	</view>
</template>

<script>
	import { carList,QQKey } from '../../static/const/api.js'
	import QQMapWX from '../../static/libs/qqmap-wx-jssdk.min.js'
	export default {
		data() {
			return {
				city:'定位中...',
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 1000,
				circular:true,
				carListDate:[],
				errSrc:require('../../static/img/noface.gif'),
				carImg:require('../../static/img/car.jpg'),//测试图片
				//carImg:require('../../static/img/svg/hot-car.svg'),//测试图片
				
				menus:[
					{
						name:"在售新车",
						icon:require('../../static/img/svg/car.svg'),
					},{
						name:"汽车保险",
						icon:require('../../static/img/svg/insurance.svg'),
					},{
						name:"优选精品",
						icon:require('../../static/img/svg/collect.svg'),
					},{
						name:"优选精品",
						icon:require('../../static/img/svg/collect.svg'),
					},
					{
						name:"在售新车",
						icon:require('../../static/img/svg/car.svg'),
					},{
						name:"汽车保险",
						icon:require('../../static/img/svg/insurance.svg'),
					},
				]
				
			}
		},
		onLoad() {
			this.getLocation();
			this.getTestCars();
		},
		methods: {
			//获取当前位置-并获取当前城市
			getLocation(){
				var _this = this;
				var qqmapsdk = new QQMapWX({
				      key: QQKey
				});
				let location={};
				uni.getLocation({
						type: 'wgs84',
						success: function (res) {
								location = {
									latitude: res.latitude,
									longitude: res.longitude
								},
								console.log(location)
								 formSubmit()
						}
				});
				//根据坐标地址逆解析出当前所在城市
				function formSubmit(e){
					qqmapsdk.reverseGeocoder({
						//位置坐标，默认获取当前位置，非必须参数
						location:location,
						//get_poi: 1, //是否返回周边POI列表：1.返回；0不返回(默认),非必须参数
						success: function(res) {//成功后的回调
							// console.log(res);
							var res = res.result;
							_this.city = res.ad_info.city;
						},
						fail: function(error) {
							console.error(error);
						},
						complete: function(res) {
							console.log(res);
						}
					})
				}
			},
			
		
			//选取试驾车
			carListClick (index,item) {
				console.log(this.carListDate[index])
			},
			//试驾车照片容错处理
			setErrorImg(e){
				console.error('image发生error事件，携带值为' + e.detail.errMsg)
			},
			getTestCars(){
				uni.showLoading({
						title: '加载中'
				});
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
							uni.hideLoading()
							this.carListDate = res.data.data;//请求接口的数据
					}
				});
			}
		}
	}
</script>

<style lang="scss">
	.content {
		height: auto;
		.header{
			padding: 0 20upx;
			display: flex;
			align-items:center;
			justify-content: space-between;
			font-size:13px; 
			height: 100upx;
			background: #ffffff;
			.search-input{
				height: 62upx;
				max-width: 620upx;
				border-radius:31upx; 
				background: #efefef;
				padding:0 20upx;
				box-sizing: border-box;
				flex:1;
				margin-left: 20upx;
			}
		}
		.swiperWrap{
			height: 385upx;
			.swiper-item{
				height: 100%;
				width: 100%;
				.car-img{
					width:100%;
				}
			}
		}
		.menu-wrap{
			background: #ffffff;
			.menu-ul{
				display: flex;
				align-items:center;
				height: auto;
				justify-content: flex-start;
				flex-wrap:wrap; 
				.menu-li{
					text-align: center;
					font-size: 28upx;
					flex: 0 0 33%;
					height: 200upx;
					display: flex;
					align-content: center;
					justify-content: center;
					padding-top:40upx;
					box-sizing: border-box;
					.menu-icon{
						width: 95upx;
						height: 95upx;
					}
				}
			}
		}
		.goods-wrap{
			background: #ffffff;
			margin-top: 20upx;
			.group{
				.group-head{
					height: 100upx;
					border-bottom: 1px solid #f8f8f8;
					vertical-align: middle;
					text-align: center;
					line-height: 100upx;
					font-size: 14px;
					position: relative;
					.group-icon{
						width: 45upx;
						height: 45upx;
						vertical-align: middle;
						margin-right: 10upx;
					}
					.group-title{
						vertical-align: middle;
					}
					.group-more{
						position: absolute;
						right: 0;
						vertical-align: middle;
						float: right;
						margin-right: 30upx;
						color: #828282;
						font-size: 13px;
						.icon-you{
							font-size:12px; 
							margin-left: 5upx;
						}
					}
				}
				.goods-ul{
					display: flex;
					flex-wrap:wrap; 
					padding: 10upx;
					box-sizing: border-box;
					justify-content:space-between;
					.goods-li{
						flex: 0 0 50%;
						overflow: hidden;
						margin-bottom: 18upx;
						padding: 10upx;
						box-sizing: border-box;
						vertical-align: baseline;
						.li-content{
							box-sizing: border-box;
							box-shadow: 0 0 2upx 1upx rgba(0,0,0,.1);
							border-radius:10upx;
							overflow: hidden;
							height: 100%;
							padding-bottom: 30upx;
							.goods-img{
								width: 100%;
								height: 345upx;
							}
							.goods-type{
								font-size:14px;
								padding: 0 20upx;
							}
							.goods-price{
								font-size:14px;
								padding: 0 20upx;
								color: red;
							}
						}

					}
				}
			}
		}
	}
</style>
