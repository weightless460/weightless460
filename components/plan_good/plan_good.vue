<template>
	<view>
		<view :class="(important==1)?'good_item2':'good_item' ">
			<view class="img_bcg">
				<image class="good_img" :src="img_url"></image>
			</view>
			<view class="good_content">
				<text class="item_name">{{good_name}}</text>
				<text class="item_introduct">{{plan_introduct}}</text>
			</view>
			<view class="show_price" v-if="!arrow">
				￥{{price}}
			</view>
			<view style="position: absolute;right: 105rpx;" v-if="arrow">
				<image src="/static/img/arrow.svg" class="arrow"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"plan_good",
		data() {
			return {
				
			};
		},
		props:['img_url','good_name','plan_introduct','price','important','arrow'],
	}
</script>

<style>
	.arrow{
		width: 40rpx;
		height: 40rpx;

	}
.good_item{
	margin-top: 20rpx;
	display: flex;
	background-color: #FFFFFF;
	height: 110rpx;
	width: 590rpx;
	border-radius: 15rpx;
	align-items: center;
	color: #000000;
}
.good_item2{
	margin-top: 20rpx;
	display: flex;
	background-color: #2EB17F;
	height: 110rpx;
	width: 590rpx;
	border-radius: 15rpx;
	align-items: center;
	color: #FFFFFF;
}
.img_bcg{
	display: flex;
	 justify-content: center; align-items: center; 
	 background-color: #7cc7d0; width: 80rpx; height: 80rpx;
	  border-radius: 30rpx;
	position: relative;
	left: 20rpx;
}
.good_img{
	width: 65rpx;
	height: 65rpx;
	border-radius: 50rpx;
}
.good_content{
	display: flex;
	flex-direction: column;
	position: absolute;
	left: 200rpx;
}

.item_name{
	font-style: normal;
	font-weight: bold;
	font-size: 28rpx;
	line-height: 36rpx;

}

.item_introduct{
	font-style: normal;
	font-weight: normal;
	font-size: 24rpx;
	line-height: 30rpx;

}

.show_price{
	font-style: normal;
	font-weight: bold;
	font-size: 28rpx;
	line-height: 36px;

	
	position: absolute;
	right: 105rpx;
}

</style>
