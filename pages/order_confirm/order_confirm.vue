<template>
	<view>
		<!-- 步骤2 的导航栏 -->
		<step_bar :step="step"></step_bar>
		<!-- 订单内容 -->
		<view class="content_bck">
			<view class="content">
				<view class="content_item" v-for="item in cert">
					<view class="good_content">
						<text class="content_name">{{item.goods_name}}</text>
						<text class="content_num">{{item.goods_price}} 元 × {{item.goods_num}} 份</text>
					</view>
					<text class="content_price">￥{{item.goods_price * item.goods_num}}</text>
				</view>
				<view style="border: 2rpx solid #979797; width: 90%; margin-left: 5%; margin-right: 5%;"></view>
				<view class="tip">
					<text class="tip_text">服务费</text>
					<text>￥5</text>
				</view>
				<view style="border: 2rpx solid #979797; width: 90%; margin-left: 5%; margin-right: 5%;"></view>
				<view class="total_price">
					<view>Total:</view>
					<text>￥{{total_price + 5}}</text>
				</view>
			</view>
		</view>
		<!-- 地址 -->
		<view style="margin-bottom: 180rpx;">
			<position></position>
		</view>
		<view class="pay">
			<image src="/static/img/pay.svg" @click="pushOrder"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 2,
				cert: [

				], // 购物车
				total_price: 0,
			}
		},
		onLoad() {
			this.cert = getApp().globalData.cert
			this.total_price = getApp().globalData.total_price
		},
		methods: {
			//获取时间函数
			getTime: function() {
				var date = new Date(),
					year = date.getFullYear(),
					month = date.getMonth() + 1,
					day = date.getDate(),
					hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours(),
					minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes(),
					second = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
				month >= 1 && month <= 9 ? (month = "0" + month) : "";
				day >= 0 && day <= 9 ? (day = "0" + day) : "";
				var timer = year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second;
				return timer;
			},
			//构造订单函数
			pushOrder: function() {
				var order = getApp().globalData.order
				order.goods_id = getApp().globalData.cert[0]//购物车内信息
				order.order_money = getApp().globalData.total_price//总价格
				order.order_time = this.getTime()//获得当前时间
				//拼接当前配送地址
				order.position = getApp().globalData.position.school+"  "+getApp().globalData.position.apartment+"  "+getApp().globalData.position.dormitory
				var show_order ="商品："+order.goods_id.goods_name+"\r\n数量："+order.goods_id.goods_num+"\r\n 价格："+order.order_money+"元  \r\n地址："+order.position
				 //确认订单，弹窗显示订单内容
				uni.showModal({
					title: '请确认订单',
					content: show_order,
					showCancel: false,
					cancelText: "取消", // 取消按钮的文字  
						confirmText: "确认", // 确认按钮的文字  
						showCancel: true, // 是否显示取消按钮，默认为 true
						confirmColor:'#39B54A' ,
						cancelColor: '#f55850',
						success: (res) => {
							if (res.confirm) {
								var that=this
								uni.request({
									url: getApp().globalData.server + '/index.php/Home/GuoFeng/test',//确认订单，发送订单信息
									data: {
										order:that.order
									},
									method: "POST",
									header: {
										'content-type': "application/x-www-form-urlencoded"
									},
									dataType: 'json',
									success: function(res) {
										console.log(res.data)
									}
								})
							}
						}
				})
			}
		}
	}
</script>

<style>
	page {
		background-color: #f6f9fc;
	}
	uni-modal .uni-modal__bd{      
	    white-space: pre-wrap;      
	}
	.pay {
		background-color: white;
		width: 100%;
		height: 150rpx;
		position: fixed;
		bottom: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 25rpx 25rpx 0 0;
	}

	.pay image {
		width: 700rpx;
		height: 100rpx;
	}

	.total_price {
		display: flex;
		position: relative;
		left: 65%;
		margin-bottom: 20rpx;
		margin-top: 20rpx;
	}

	.total_price view {
		font-style: normal;
		font-weight: normal;
		font-size: 18px;
		line-height: 23px;
		/* identical to box height */

		text-align: center;
		letter-spacing: -0.3px;

		color: #000000;
	}

	.total_price text {
		font-style: normal;
		font-weight: bold;
		font-size: 18px;
		line-height: 23px;
		/* identical to box height */

		text-align: center;
		letter-spacing: -0.3px;

		color: #000000;
	}

	.tip {
		display: flex;
		justify-content: space-between;
		margin: 5%;
	}

	.tip_text {
		font-style: normal;
		font-weight: bold;
		font-size: 18px;
		line-height: 23px;
		/* identical to box height */

		text-align: center;
		letter-spacing: -0.3px;

		color: #000000;
	}

	.content_bck {
		width: 100%;
		display: flex;
		justify-content: center;
	}

	.content {
		width: 680rpx;
		background-color: white;
		margin-top: 40rpx;
		border-radius: 25rpx;
	}

	.content_item {
		display: flex;
		justify-content: space-between;
		margin: 35rpx;
	}

	.good_content {
		display: flex;
		flex-direction: column;
	}

	.content_name {
		font-style: normal;
		font-weight: bold;
		font-size: 18px;
		line-height: 23px;
		/* identical to box height */

		letter-spacing: -0.3px;

		color: #000000;

	}

	.content_num {
		font-style: normal;
		font-weight: normal;
		font-size: 28rpx;
		line-height: 40rpx;
		/* identical to box height */

		letter-spacing: -0.3px;

		color: #979797;
	}

	.content_price {
		font-style: normal;
		font-weight: bold;
		font-size: 18px;
		line-height: 23px;
		/* identical to box height */

		text-align: center;
		letter-spacing: -0.3px;

		color: #000000;
	}
</style>
