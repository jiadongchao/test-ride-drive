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
		<view>
			<ul class="menu-ul">
				<li class='menu-li' v-for="(item,index) in menus" :key="item.index" @click="menuClick(index)">
					<view>
						<image class="menu-icon" :src="item.icon" v-if="item.icon" mode="aspectFit"></image>
						<view>{{item.name}}</view>
					</view>
				</li>
			</ul>
		</view>
		<!-- goods -->
		<view class="goosWrap">
			
		</view>
		
	</view>
</template>

<script>
	import { carList,QQKey } from '../../static/const/api.js'
	import QQMapWX from '../../static/libs/qqmap-wx-jssdk.min.js'
	export default {
		data() {
			return {
				city:"",
				indicatorDots: true,
				autoplay: false,
				interval: 2000,
				duration: 1000,
				circular:true,
				carListDate:[],
				errSrc:require('../../static/img/noface.gif'),
				carImg:require('../../static/img/car.jpg'),//测试图片
				
				menus:[
					{
						name:"在售新车",
						icon:require('../../static/img/icon-menu/car.png'),
					},{
						name:"汽车保险",
						icon:require('../../static/img/icon-menu/insurance.png'),
					},{
						name:"优选精品",
						icon:require('../../static/img/icon-menu/googs.png'),
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
		.header{
			padding: 0 20upx;
			display: flex;
			align-items:center;
			justify-content: space-between;
			font-size:25upx; 
			height: 100upx;
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
		.menu-ul{
			display: flex;
			align-items:center;
			height: 200upx;
			justify-content: space-around;
			.menu-li{
				text-align: center;
				font-size: 28upx;
				.menu-icon{
					width: 95upx;
					height: 95upx;
				}
			}
		}
	}
</style>
