<template>
	<view>
		<view class="cont">
			<swiper class="swiper" @change="fenimg">
				<swiper-item v-for="(item,index) in imgsdata" class="swiper-item" v-bind:key="index">
					<image class="swiper-img" :src="(item)"></image>
				</swiper-item>
			</swiper>
			<view class="num">
				{{num}}/{{imgslen}}
			</view>
		</view>
		<view class="pricecont">
			<view class="price">
				<text class="priq">￥</text>
				<text class="pric">{{price}}</text>
			</view>
			<view class="pricc">
				卷后<text class="priq">￥</text>12399
			</view>
			<view class="zhanwei">
			</view>
			<view class="depreciate">
				<view class="iconfont depicon icon-jiangjiatixing"></view>
				<view class="fonntsize">降价提醒</view>
			</view>
			<view class="">
				<view class=" iconfont icon-aixinjiaguanzhu"></view>
				<view class="fonntsize">收藏</view>
			</view>
			<!-- </view> -->
		</view>
		<view class="shoppname">
			<text class="ziying">自营</text>
			{{datas.good_name.name}}
		</view>
		<view class="pricecont">
			<view class="">
				已选
			</view>
			<view class="alllist">
				<text v-for="(item,index) in listdata" :key="index">{{item}},</text>
				<text>{{nums}}个</text>
				可选服务
			</view>
			<view class="shenglve" @click="shenglve">
				<text class="iconfont deticon icon-shenglvehao"></text>
			</view>
		</view>
		<view class="pricecont">
			<view class="">
				送至
			</view>
			<view class="alllist">
				{{address}}
			</view>
			<view class="shenglve">
				<text class="iconfont deticon icon-shenglvehao"></text>
			</view>
		</view>
		<view class="shade" v-show="shade">
			<view class="sshade">
				<view class="quxiao left">

					<view class="iconfont icons icon-a-quxiao" @click="quxiao"></view>
				</view>
				<view class="pricecont">
					<view class="">
						<image class="imgs" :src="(datas.img)"></image>
					</view>
					<view class="">
						<view class="left">
							<view class="price">
								<text class="priq">￥</text>
								<text class="pric">{{total}}</text>
							</view>
							<view class="pricc">
								卷后<text class="priq">￥</text>12399
							</view>
						</view>
						<view class="alllist">
							已选
							<text v-for="(item,index) in listdata" :key="index">{{item}},</text>
							<text>{{nums}}个</text>
						</view>
					</view>
				</view>
				<view class="shoppname">
					<view class="" v-for="(item, index) in edition.specList " :key='index'>
						<view class="">
							{{item.titel}}
						</view>
						<view class="sitem2">
							<view class="item2" v-for="(item2,indexs) in item.list"
								@click="clickspu(item2,index,indexs)" :key='indexs'
								:class="skunull[index]===item2 ?'comboitems':'comboitem'">
								{{item2}}
							</view>
						</view>
					</view>
				</view>
				<view class="shoppname sitem2 ">
					<view class="zhanwei">
						数量
					</view>
					<view class="sitem2 ">
						<view class="" @click="sub">
							-
						</view>
						<input class="uni-input" type="number" @input="onKeyInput" :value="(nums)" />
						<view class="" @click="sup">
							+
						</view>
					</view>
				</view>
				<view class="shoppname sitem2">
					<!-- 					<view class="shopping">
						加入购入车
					</view> -->
					<view class="buy" @click="confirm">
						确定
					</view>
				</view>
			</view>
		</view>
		<view class="bottom">
			<view class="icon zhanwei">
				<text class="iconfont icon gicon icon-dianpu"></text>
			</view>
			<view class="icon zhanwei">
				<text class="iconfont icon gicon icon-kefu"></text>
			</view>
			<view class="icon cont zhanwei">
				<text class="iconfont icon gicon icon-gouwuche1" @click="subcar"></text>
				<view class="shoppnum">
					{{shoppingcarlen}}
				</view>
			</view>
			<view class="shopping zhanwei" @click="shoppcar">
				加入购物车
			</view>
			<view class="buy marg zhanwei">
				立即购买
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgsdata: [],
				num: 1,
				nums: 1,
				imgslen: 1,
				price: 0,
				datas: [],
				edition: [],
				listdata: [],
				skunull: [],
				skuList: [],
				address: "四川省武侯区孵化园9号楼e座",
				skudata: [],
				skuspri: '',
				shade: false,
				shoppingcarlen: '',
				shoppingdata: [],
				good_id: '',
				shopping_car_id: '',
				total: '', //总价
				sku: '',
			}
		},
		// computed: {
		// 	skucomput(item2){
		// 		for(let i=0;i<this.skunull.length;i++){
		// 		return this.skunull[i]==item2?'comboitems':'comboitem'
		// 		}
		// 	}
		// },
		methods: {
			quxiao() {
				console.log(12345)
				this.shade = false
			},
			fenimg(e) {
				this.num = e.detail.current + 1
			},
			shenglve() {
				this.shade = true
				console.log(12324)
			},
			clickspu(item, indexs, index) {
				console.log(item, indexs, index, this.sku)
				this.skunull[indexs] = item
				let s = ''
				this.skunull.forEach(item => {
					s += item + ","
				})
				this.sku = s
				// console.log(s)
				this.listdata = this.skunull
				console.log(this.skunull, 9999999)
				this.$forceUpdate()
				console.log(this.skuList, 0)
				for (let i = 0; i < this.skuList.length; i++) {
					if (this.isContained(this.skuList[i].specs, this.skunull)) {
						this.skudata = this.skuList[i].specs
						let len = this.skuList[i].specs.length
						this.skuspri = this.skuList[i].specs[len - 1]
						console.log(this.skuspri)
					}
				}
			},
			isContained(a, b) {
				if (!(a instanceof Array) || !(b instanceof Array)) return false;
				if (a.length < b.length) return false;
				var aStr = a.toString();
				for (var i = 0, len = b.length; i < len; i++) {
					if (aStr.indexOf(b[i]) == -1) return false;
				}
				return true;
			},
			onKeyInput(e) {
				let num = e.target.value
				if (Number(e.target.value) <= 200 && Number(e.target.value) >= 1) {
					this.nums = Number(num)
					if (this.nums == 1) {
						console.log(987)
						this.nums = num
						this.total = this.skuspri
						this.$forceUpdate()
					}
					this.total = this.skuspri * this.nums
				} else {
					num = 1
					this.nums = num
					console.log(this.nums, 123456)
					this.$forceUpdate()
				}
			},
			sub() {
				this.nums--
				if (this.nums < 1) {
					this.nums = 1
				}
				this.total = this.skuspri * this.nums
			},
			sup() {
				this.nums++
				if (this.nums > 200) {
					this.nums = 200
				}
				this.total = this.skuspri * this.nums
			},
			subcar() {
				uni.reLaunch({
					url: '/pages/shoppingcar/shoppingcar'
				})
			},
			request() {
				let than = this
				if (!uni.getStorageSync("token")) {
					uni.showModal({
						title: '提示',
						content: '请登录',
						success: function(res) {
							if (res.confirm) {
								uni.navigateTo({
									url: '/pages/login/login'
								});
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
				}
				uni.request({
					url: 'http://api_devs.wanxikeji.cn/api/shoppingCarList',
					data: {
						token: uni.getStorageSync("token")
					},
					success(res) {
						// console.log(res.data.data.data)
						than.shoppingdata = res.data.data.data
						than.shoppingcarlen = res.data.data.count
					}
				})
			},
			shoppcar() {
				this.shade = true
			},

			confirm() {
				let than = this
				// console.log(this.shoppingdata)
				this.shoppingdata.forEach(item => {
					if (this.good_id == item.good_id) {
						this.shopping_car_id = item.shopping_car_id
					}
				})
				// console.log(this.shopping_car_id)
				uni.request({
					url: 'http://api_devs.wanxikeji.cn/api/shoppingCarAddModify',
					data: {
						token: uni.getStorageSync("token"),
						good_id: this.good_id, //商品id
						num: this.nums, //数量
						price: this.skuspri, //单价
						money: this.total, //总价
						sku: this.sku, //	sku
						shopping_car_id: this.shopping_car_id //	购物车id(修改时必传)
					},
					success(res) {
						// console.log(res, 123)
						uni.showToast({
							title: res.data.msg,
							duration: 2000
						});
						than.request()
						than.shade = false
						// than.shoppingcarlen=res.data.data.count
					}
				})
			},
		},
		// beforeCreate(){
		// 	this.request()
		// },
		onLoad(opctions) {
			let a = JSON.parse(opctions.a)
			this.good_id = a.good_id
			// console.log(a.good_id,4)
			this.price = a.price
			uni.request({
				url: 'http://api_devs.wanxikeji.cn/api/goodInfo',
				data: {
					good_id: a.good_id
				},
				success: (res) => {
					let imgs = JSON.parse(res.data.data.info[0].imgs)
					if (imgs.indexOf(',') > 0) {
						imgs = imgs.split(",")
						this.imgslen = imgs.length
					}
					this.imgsdata = imgs
					res.data.data.good_name = JSON.parse(res.data.data.good_name)
					res.data.data.info[0].colour = JSON.parse(res.data.data.info[0].colour)
					res.data.data.info[0].edition = JSON.parse(res.data.data.info[0].edition)
					res.data.data.info[0].info = JSON.parse(res.data.data.info[0].info)
					this.datas = res.data.data
					this.edition = JSON.parse(res.data.data.info[0].edition)
					let a = JSON.parse(res.data.data.info[0].edition)
					let len = a.skuList[0].specs.length
					this.skuspri = this.edition.skuList[0].specs[len - 1]
					this.total = this.skuspri
					this.skuList = this.edition.skuList
					this.listdata = a.skuList[0].specs.splice(0, len - 2)
					let s = ''
					this.listdata.forEach((ele) => {
						this.skunull.push(ele)
						s += ele + ","
					})
					this.sku = s
					console.log(res.data.data, this.edition, this.listdata, len)
				}
			})
			this.request()
			// api/shoppingCarDelete
			// uni.request({
			// 	url: 'http://api_devs.wanxikeji.cn/api/shoppingCarDelete',
			// 	data: {
			// 		shopping_car_id: 4560,
			// 		token: uni.getStorageSync("token"),
			// 	},
			// 	success(res) {
			// 	console.log(res)	
			// 	}})
		}
	}
</script>

<style>
	.cont {
		position: relative
	}

	.swiper {
		width: 100%;
		height: 400rpx;
		/* position: ; */
	}

	.swiper-item {
		width: 100%;
		height: 400rpx;
	}

	.swiper-img {
		width: 100%;
		/* height: auto; */
		height: 400rpx;
	}

	.num {
		position: absolute;
		bottom: 40rpx;
		right: 0;
		color: #FFFFFF;
		padding: 5rpx 20rpx;
		background-color: rgba(0, 0, 0, .5);
		border-radius: 25rpx 0 0 25rpx;
	}

	.pricecont {
		display: flex;
		padding: 20rpx 30rpx;
	}

	.fonntsize {
		font-size: 20rpx;
	}

	.icon {
		text-align: center;
		justify: center;
		align-items: center;
		display: -webkit-flex
	}

	.price {
		color: #FF4142;
		margin-right: 20rpx;
		justify: center;
		align-items: center;
		display: -webkit-flex
	}

	.priq {
		font-size: 20rpx;
	}

	.pricc {
		background-color: #FF4142;
		color: #FFFFFF;
		border-radius: 10rpx;
		justify: center;
		align-items: center;
		display: -webkit-flex;
		padding: 0 10rpx;
	}

	.ziying {
		background-color: #FF4142;
		color: #FFFFFF;
		padding: 5rpx;
		border-radius: 5rpx;
		margin-right: 10rpx;
		font-size: 20rpx;
	}

	.zhanwei {
		flex-grow: 1;
	}

	.depreciate {
		margin: 0 20rpx;
	}

	.shoppname {
		padding: 0 20rpx;
	}

	.alllist {
		flex-grow: 1;
		padding: 0 20rpx;
	}

	.shenglve {
		justify: center;
		align-items: center;
		display: -webkit-flex
	}

	.shade {
		position: fixed;
		bottom: 0;
		top: 0;
		left: 0;
		right: 0;
		z-index: 999;
		background-color: rgba(0, 0, 0, .5);
	}

	.sshade {
		background-color: #FFFFFF;
		position: fixed;
		bottom: 0;
		width: 100%;
	}

	.left {
		display: flex;
	}

	.imgs {
		width: 100rpx;
		height: 100rpx;
	}

	.sitem2 {
		display: flex;
		flex-wrap: wrap;
	}

	.comboitems {
		background-color: #FCEDEB;
		border: 1px solid #F2280E;
		color: #F2280E;
		padding: 10rpx 30rpx;
		margin: 15rpx 50rpx 20rpx 0;
		border-radius: 30rpx;
	}

	.comboitem {
		padding: 10rpx 30rpx;
		margin: 15rpx 50rpx 20rpx 0;
		border-radius: 30rpx;
		background-color: #F2F2F2;
	}

	.uni-input {
		width: 60rpx;
		margin: 0 20rpx;
		background-color: #8F8F94;
	}

	.shopping {
		background-color: #FFC70D;
		color: #fff;
		font-size: 30rpx;
		width: 200rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 20rpx 0;
		border-radius: 50rpx;
		/* margin-right: 10rpx; */
		margin: 20rpx auto;
	}

	.buy {
		color: #fff;
		font-size: 30rpx;
		width: 200rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 20rpx 0;
		border-radius: 50rpx;
		background-color: #F2310C;
		margin: 20rpx auto;
	}

	.quxiao {
		padding: 20rpx;
		justify-content: flex-end;
		/* text-align: right; */
	}

	.icons {
		width: 35rpx;
		height: 35rpx;
	}

	.bottom {
		position: fixed;
		display: flex;
		bottom: 0;
		width: 100%;
		background-color: #F8F8F8;
		padding: 0 20rpx;
		box-sizing: border-box;
	}

	.gicon {
		font-size: 50rpx;
	}

	.marg {
		margin: 20rpx 0 20rpx 20rpx;
	}

	.shoppnum {
		position: absolute;
		top: 0;
		right: 0;
		padding: 1rpx 12rpx;
		color: #F2310C;
		border-radius: 50%;
		border: 1rpx solid #F2310C;
	}

	.deticon {
		font-size: 15rpx;
	}

	.depicon {
		text-align: center;
		justify: center;
		align-items: center;
	}
</style>
