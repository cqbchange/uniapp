<template>


	<view class="accmain">
		<view class="acctop">
			<view class="acctitle">
				<view class="accadd">
					<view class="">
						<text>陈庆博</text>
						<text>13273021913</text>
					</view>
					<view class="">
						四川省武侯区孵化园9号楼e座
					</view>
				</view>
				<view class="accyou">
					<text class="iconfont accicon icon-mjiantou-copy"></text>
				</view>
			</view>
			<view class="">
				<image class="imgcai" src="../../static/caidai.svg"></image>
			</view>
		</view>
		<view class="acccont">
			<view class="">JD
				京东自营
			</view>
			<view v-for="(item,index) in accdata" :key="index" class="shopptitle shopplabel">

				<view class="">
					<image :src="(item.img)" class="shoppimg"></image>
				</view>
				<view class="shoptext">
					<view class="shoppheade">{{item.good_name.name}}</view>
					<view class="shoppsku">{{item.sku}}</view>
					<view class="shopptitle">
						<view class="shopptitle shopppric">
							<text accpri>￥</text> <text class="accpric">{{item.price}}</text>
						</view>
						<view class="shopptitle">
							X{{item.num}}
						</view>
					</view>
					<view class="">
						<text class="iconfont iconcolor accfont icon-kongxinyuan7"></text>
						<text class="accfont">支持7天无理由退货（合约版不支持</text>
					</view>
					<view class="">
						<text class="iconfont iconcolor accfont icon-gantanhao"></text>
						<text class="accfont"> 价格说明 </text>
					</view>
				</view>
			</view>
			<view class="shopptitle shoppsku">
				<view class="">
					配送
				</view>
				<view class="reight">
					<view class="">
						京东快递
					</view>
					<view class="">
						工作日，双休日与节假日均可配送
					</view>
				</view>
				<view class="accyou">
					<text class="iconfont accicon icon-mjiantou-copy"></text>
				</view>
			</view>
		</view>
		<view class="freight">
			<view class="accpodding">
				<view class="accadd">商品总额</view>
				<view class="right">￥13948.00</view>
			</view>
			<view class="accpodding">
				<view class="accadd"> <text>运费</text><text class="">（总重：5.210kg）</text> </text>
				</view>
				<view class="right">免费</view>
			</view>
			<view class="accpodding">
				<view class="accadd">
					<view> <text>优惠卷 </text><text>已选推荐优惠</text></view>
				</view>
				<view class="right">-￥100.00</view>
				<view class="iconfont accicon right icon-mjiantou-copy"></view>
			</view>
			<view class="accpodding">
				<view class="juzhong">
				<text class=" depicon ">礼品卡，红包咱不可用  </text>	
				<text class="iconfont accicon depicon  icon-xiala2"></text>
				</view>
			</view>
		</view>
		<view class="accpririg allpric">
			总价：￥{{price}}
		</view>
		<view class="accpodding accmarig" accyou>
		<view class="accadd ">发票</view>
		<view class="accyou">			
			<text class="depicon  accpri">电子发票（商品明细-个人）</text>
			<text class="iconfont accicon icon-mjiantou-copy"></text>
		</view>
		</view>
		<view class="">
			<label >
				<view class="accpodding accyou">
					<view class="accadd">
					微信支付
					</view>
					<view class="">
					<checkbox :checked="(item.checked)" @click="select(item,index)" />
					</view>
				</view>
			</label>
			<label class="">
				<view class="accpodding accyou">
					<view class="accadd">
					找人支付
					</view>
					<view class="">			
					<text>指定一位好友帮您付款</text>
					<checkbox :checked="(item.checked)" @click="select(item,index)" />
					</view>
				</view>
			</label>
		</view>
		<view class="accbottom allpric accyou">
			<view class="accadd">
				总价：￥{{price}}
			</view>
			<view class="pay" @click="pay">
				微信支付
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				accdata: [],
				price:0,
				addid:0,
				shoppdatas:[]
			}
		},
		methods: {
pay(){
	
	console.log(this.addid,this.price,this.shoppdatas)
	uni.request({
		url: 'http://api_devs.wanxikeji.cn/api/generateOrder',
		 method: "post",
		data: {
			token: uni.getStorageSync("token"),
			address_id:String(this.addid),
			money:String(this.price),
			shopping_car_ids:this.shoppdatas
		},
		success(res) {
			let s=res.data.data.prepay_id
			console.log(res.data.data.prepay_id,res)
			uni.requestPayment({
			    // provider: 'wxpay',
			    timeStamp: res.data.data.timeStamp,
			    nonceStr: res.data.data.nonce_str,
			    package: 'prepay_id='+s,
			    signType: 'MD5',
			    paySign: res.data.data.paySign,
			    success: function (ress) {
			        console.log('success:' + JSON.stringify(ress));
			    },
			    fail: function (err) {
			        console.log('fail:' + JSON.stringify(err));
			    }
			});
		}
	})
	
	
	
	
	
},add(){
	let than=this
	uni.request({
		url: 'http://api_devs.wanxikeji.cn/api/userAddressList',
		data: {
			token: uni.getStorageSync("token"),
		},
		success(res) {
			console.log(res.data.data[0])
			than.addid=res.data.data[0].address_id
		}
	})
}
		},
		
		onShow(){
this.add()
		},
		onLoad(opctions) {
			if (opctions) {
				let arr = JSON.parse(opctions.a)
				console.log(arr)
				this.accdata = arr.arr
				this.price=arr.shoppprice
				this.shoppdatas=arr.arrcarids
			}
		}
	}
</script>

<style>
	.accmain {
		padding-top: 1rpx;
		background-color: #F4F4F4;
		margin-bottom: 120rpx;
	}

	.acctitle {
		display: flex;
		padding: 20rpx;
	}

	.acccont {
		margin: 200rpx 0rpx 0rpx;
		padding: 0 20rpx;
		background-color: #FFFFFF;
	}

	.imgcai {
		width: 100%;
		height: 20rpx;
	}

	.accyou {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.accicon {
		font-size: 50rpx;
	}

	.accadd {
		flex-grow: 1;
	}

	.shopptitle {
		display: flex;
		background-color: #FFFFFF;
	}

	.shopplabel {
		padding: 20rpx 0;
		border-bottom: 1rpx solid rgba(80, 80, 80, .5);
	}

	.shoppimg {
		width: 200rpx;
		height: 200rpx;
	}

	.shoptext {
		margin-left: 20rpx;
		flex-grow: 1;
	}

	.shoppheade {
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
	}

	.headetext {
		background-color: #FF4142;
		color: #FFFFFF;
		padding: 0 10rpx;
		margin-right: 10rpx;
	}

	.shoppsku {
		padding: 20rpx 0;
	}

	.shopppric {
		flex-grow: 1;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 1;
		color: #FF4142;
	}

	.accpri {
		font-size: 20rpx;
	}

	.accfont {
		font-size: 20rpx;
		padding-left: 20rpx;
	}

	.iconcolor {
		color: #007AFF;
	}

	.reight {
		flex-grow: 1;
		text-align: right;
		font-size: 20rpx;
	}

	.acctop {
		position: fixed;
		top: 0;
		width: 100%;
		z-index: 99;
		background-color: #FFFFFF;
	}

	.accbottom {
		display: flex;
		position: fixed;
		bottom: 0;
		width: 100%;
	}

	.freight {
		margin-top: 20rpx;
		border-bottom: 1rpx solid rgba(90, 90, 90, .5);
	}

	.accpodding {
		padding: 20rpx;
		display: flex;
		background-color: #FFFFFF;
	}

	.juzhong {
		margin: 0 auto;
	}
	
	.depicon {
	    text-align: center;
	    justify: center;
	    -webkit-box-align: center;
	    -webkit-align-items: center;
	    align-items: center;
		}
		.accpririg{
			text-align: right;
			margin-bottom: 20rpx;
		}
		.allpric{
			padding: 20rpx;
			background-color: #FFFFFF;
		}
		.accmarig{
			margin-bottom: 20rpx;
		}
		.accbottom{
			position: fixed;
			border-bottom: 0;
			width: 100%;
			box-sizing: border-box;
		}
		.pay{
			padding: 20rpx;
			background-color: #FF4142;
			border-radius: 44rpx;
		}
</style>
