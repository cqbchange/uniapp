<template>
	<view class="">
		<view class="indedxtop">
			<view class="topdata">
				<view class="topitem" v-for="(item,index) in topdata" :key='index' @click="topitem(index)">
					<view class="juzhong">
						<text class="juzhong">{{item.name}}</text>
						<text class="iconfont  juzhong" :class='item.icon'></text>
					</view>
					<view :class='[action==index?"juzhong":"stylecolor"]'>
						<text class="iconfont juzhong huicon icon-huxing"></text>
					</view>
				</view>
			</view>
			<view class="topdata">
				<view class="topitem topstyle" v-for="(item,index) in toplist" :key='index'>
					<view class="juzhong">
						<text class="juzhong">{{item.name}}</text>
						<text class="iconfont  juzhong" :class='item.icon'></text>
					</view>
				</view>
			</view>
			<view class="topright" @click="toprights">
				筛选
				<text class="iconfont icon-shaixuan"></text>
			</view>
		</view>

		<view class="main">
			<view class="content" v-for="(item,index) in arrdata" v-bind:key='index' @click="menuClick(item)">
				<image class="images" :src="(item.img)"></image>
				<view class="name"> {{item.good_name.name}}</view>
				<view class="goodspric"> <text class="goodicon">￥</text>{{item.price}}</view>
				<view class="zfj">
					<view class="ziying">
						自营
					</view>
					<view class="xinping">
						新品
					</view>
					<view class="juan">
						秒杀
					</view>
				</view>
				<view class="haoping">10万+评论 95%好评</view>
				<view class="dian">
					<view class="jddian">
						京东自营店
					</view>
					<view class="text">
						进店
						<text class="iconfont icon-mjiantou-copy"></text>
					</view>
				</view>
			</view>
		</view>
		<view class="indexshade" v-if="topright" @click="click">
			<view class="bottleft" @click.stop>
				<view class="shadetop">
					<view class="itemwarp">配送至 </view>
					<view>四川省武侯区孵化园9号楼e座</view>
					<view class="iconfont icon icon-mjiantou-copy"></view>
				</view>
				<view class="">
					<view class="itemflex">
						<view class="itemflex juzhong itemcolor" v-for="(item,index) in fendata" :key='index'>
							{{item}}
						</view>
					</view>
				</view>
				<view class="shadetop">
					<view class="itemwarp">分类 </view>
					<view>全部分类</view>
					<view class="iconfont icon icon-mjiantou-copy"></view>
				</view>
				<view class="ground">
					<view class="">价格 </view>
					<view class="itemflex">
						<view class="itemflex icon itemcolor" v-for="(item,index) in fendata" :key='index'>
							{{item}}
						</view>
					</view>
				</view>
				<view class="ground">
					<view class="">品牌</view>
					<view class="itemflex">
						<view class="itemflex juzhong itemcolor" v-for="(item,index) in fendata" :key='index'>
							{{item}}
						</view>
					</view>
				</view>
				<view class="ground">
					<view class="">CPU型号</view>
					<view class="itemflex">
						<view class="itemflex juzhong itemcolor" v-for="(item,index) in fendata" :key='index'>
							{{item}}
						</view>
					</view>
				</view>
				<view class="ground">
					<view class="">机身存储</view>
					<view class="itemflex">
						<view class="itemflex juzhong itemcolor" v-for="(item,index) in fendata" :key='index'>
							{{item}}
						</view>
					</view>
				</view>
				<view class="ground">
					<view class="">运行内存</view>
					<view class="itemflex">
						<view class="itemflex juzhong itemcolor" v-for="(item,index) in fendata" :key='index'>
							{{item}}
						</view>
					</view>
				</view>
				<view class="ground">
					<view class="topdata xians">
						<view class="">
							热门机型
						</view>
						<view class="topover">
							<text v-for="(item,index) in topcolorlist" :key="index">{{item.name}}</text>
						</view>
						<view class="">
							{{gong}}
						</view>
					</view>
					<view class="itemflex">
						<view class="itemflex juzhong " v-for="(item,index) in fendatas" :key='index'
							@click="selectdata(item,index)" :class="item.talg==true?'topselect':'itemcolor'">
							{{item.name}}
						</view>
					</view>
				</view>
				<view class="topbottoms">
					<view class="botpadding botborder">
						重置
					</view>
					<view class="botpadding botcolor">
						确定
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				gdata: 5000,
				arrdata: [],
				action: 0,
				index: '',
				topright: false,
				topcolorlist: [],
				topdata: [{
					name: '综合',
					icon: 'icon-xiala2'
				}, {
					name: '销量',
					icon: 'icon-xiala2'
				}, {
					name: '价格',
					icon: 'icon-xiala2'
				}, {
					name: '',
					icon: 'icon-xiaoshida'
				}, ],
				toplist: [{
					name: '京东物流',
					icon: ''
				}, {
					name: '新品',
					icon: ''
				}, {
					name: '品牌',
					icon: 'icon-xiala2'
				}, {
					name: '拍拍二手',
					icon: ''
				}, ],
				fendata: [
					'京东物流',
					'有货优先',
					'货到付款',
					'211限时达',
					'新品',
					'京东国际',
					'PULUS专享',
					'促销商品',
					'拍怕二手',
				],
				fendatas: [{
						name: '京东物流',
						talg: false
					}, {
						name: '有货优先',
						talg: false
					}, {
						name: '货到付款',
						talg: false
					}, {
						name: '211限时达',
						talg: false
					}, {
						name: '新品',
						talg: false
					}, {
						name: '京东国际',
						talg: false
					}, {
						name: 'PULUS专享',
						talg: false
					}, {
						name: '促销商品',
						talg: false
					}, {
						name: '拍怕二手',
						talg: false
					},

				],
				gong: ''
			}
		},
		onLoad() {
			uni.request({
				// api/goodInfo
				url: 'http://api_devs.wanxikeji.cn/api/goodList',
				data: {
					size: this.gdata
				},
				success: (res) => {
					console.log(res.data.data.data);
					let list = res.data.data.data
					let arr = []
					for (let i = 0; i < list.length; i++) {
						if (list[i].good_name.indexOf("lsycqb") >= 0) {
							list[i].good_name = JSON.parse(list[i].good_name)
							arr.push(list[i])
						}
					}
					console.log(arr,123)
					this.arrdata = arr
				}
			})
		},
		methods: {
			selectdata(item, index) {
				let arr = []
				arr.push(item)
				this.index = index
				if (this.topcolorlist.length < 6) {
					this.topcolorlist.push(item)
					let s = new Set(this.topcolorlist)
					if (item.talg == false) {
						item.talg = true

					} else {
						item.talg = false
					}
					// let s= new Set(this.topcolorlist)
					let arr = []
					this.fendatas.forEach(items => {
						if (items.talg == true) {
							arr.push(items)
						}
					})
					// this.topcolorlist = Array.from(s)	
					this.topcolorlist = arr
					this.gong = '共' + this.topcolorlist.length + '个'
				} else {
					item.talg = false
					let s = new Set(this.topcolorlist)
					let arr = []
					this.fendatas.forEach(items => {
						if (items.talg == true) {
							arr.push(items)
						}
					})
					this.topcolorlist = arr
					this.gong = '共' + this.topcolorlist.length + '个'
				}

			},

			toprights() {
				console.log(234)
				this.topright = true
			},
			topitem(index) {
				this.action = index
			},
			click(e) {
				this.topright = false
			},
			menuClick(item) {
				// console.log(item)
				let a = JSON.stringify(item)
				uni.navigateTo({
					url: '/pages/details/details?a=' + a
				})
			}
		},
	}
</script>

<style>
	.xians {
		padding: 20rpx 0;
	}

	.topover {
		width: 60%;
		padding-left: 10rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		height: 50rpx;
		display: -webkit-box;

		-webkit-box-orient: vertical;

		-webkit-line-clamp: 1;
	}

	.topselect {
		background-color: #FCEDEB;
		border: 1px solid #F2280E;
		color: #F2280E;
		padding: 8rpx 18rpx;
		margin: 10rpx auto;
		/* padding: 10rpx 30rpx; */
		/* margin: 15rpx 50rpx 20rpx 0; */
		border-radius: 55rpx;
	}

	.botborder {
		border: 1rpx solid #999999;
	}

	.botcolor {
		background-color: #F2280E;
		color: #FFFFFF;
	}

	.botpadding {
		/* flex-grow: 1; */
		/* width: 200rpx; */
		/* padding: ; */
		padding: 20rpx 100rpx;
		margin: 0 auto;
		border-radius: 55rpx;

	}

	.topbottoms {
		display: flex;
		position: fixed;
		bottom: 0;
		left: 100rpx;
		right: 0;
		/* width: 100%; */
		padding: 20rpx;
		box-sizing: border-box;
		background-color: #FFFFFF;
	}

	.ground {
		background-color: #FFFFFF;
		padding: 20rpx;
		margin-bottom: 20rpx;
	}

	.shadetop {
		display: flex;
		margin-bottom: 20rpx;
		background-color: #FFFFFF;
		padding: 20rpx;
	}

	.bottleft {
		margin-left: 100rpx;
		width: calc(100% - 100rpx);
		background-color: #F4F4F4;
		height: 100%;
		/* padding: 20rpx; */
		box-sizing: border-box;
		overflow: scroll;
		padding-bottom: 80rpx;
	}

	.icon {
		text-align: center;
		justify: center;
		align-items: center;
		display: -webkit-flex
	}

	.itemwarp {
		flex-grow: 1;
	}

	.itemflex {
		display: flex;
		flex-wrap: wrap;
		background-color: #FFFFFF;
		margin-bottom: 20rpx;
	}

	.itemcolor {
		background-color: #F4F4F4;
		border-radius: 55rpx;
		padding: 10rpx 20rpx;
		margin: 10rpx auto;
	}

	.indexshade {
		position: fixed;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		background-color: rgba(80, 80, 80, .5);
		z-index: 999999;
		/* margin-bottom: 100rpx; */
	}

	.topright {
		position: fixed;
		right: 0;
		top: 63rpx;
		padding: 10rpx;
		font-size: 24rpx;
		background-color: #FFFFFF;
		z-index: 99;
		box-shadow: 1rpx 0rpx 1rpx #F1F1F1;
	}

	.topstyle {
		background-color: #F4F4F4;
		border-radius: 50rpx;
		margin: 0 10rpx;
		font-size: 20rpx;
		text-align: center;
	}

	.stylecolor {
		display: none;
	}

	.huicon {
		font-size: 14rpx;
		color: #FF4142;
	}

	.topdata {
		display: flex;
		background-color: #FFFFFF;
	}

	.topitem {
		flex-grow: 1;
		padding-left: 34rpx;
	}

	.main {
		display: flex;
		flex-wrap: wrap;
		background-color: #F4F4F4;
		justify-content: space-between;
		padding: 20rpx;
		margin-top: 140rpx;
	}

	.indedxtop {
		position: fixed;
		top: 0;
		width: 100%;
		padding: 20rpx;
		background-color: #FFFFFF;
		box-sizing: border-box;
	}

	.juzhong {
		justify: center;
		align-items: center;
		display: -webkit-flex;
	}

	.content {
		width: 48%;
		margin-bottom: 20rpx;
		background-color: #fff;
		padding-bottom: 20rpx;
	}

	.images {
		width: 100%;
		height: 290rpx;
		display: block;
	}

	.name {
		overflow: hidden;
		padding: 0 20rpx;
		text-overflow: ellipsis;

		display: -webkit-box;

		-webkit-box-orient: vertical;

		-webkit-line-clamp: 2;
	}

	.goodspric {
		font-size: 38rpx;
		font-weight: bold;
		margin-top: 10rpx;
		color: #FF4142;
		display: flex;
		align-items: center;
		padding: 0 20rpx;
	}

	.goodicon {
		font-size: 24rpx;
	}

	.zfj {
		display: flex;
		font-size: 24rpx;
		padding: 20rpx 20rpx 0;
	}

	.ziying {
		background-color: #FF4142;
		color: #FFFFFF;
		padding: 5rpx;
		border-radius: 5rpx;
	}

	.xinping {
		color: #31C19E;
		border: 1px solid #31C19E;
		padding: 5rpx;
		margin: 0 20rpx;
		border-radius: 5rpx;
	}

	.juan {
		border: 1rpx solid #FF4142;
		color: #FF4142;
		padding: 5rpx;
		border-radius: 5rpx;
	}

	.haoping {
		font-size: 24rpx;
		padding: 20rpx 20rpx;
	}

	.dian {
		font-size: 24rpx;
		padding-left: 20rpx;
		display: flex;
		flex-wrap: nowrap;
		line-height: 40rpx;
	}

	.jddian {
		width: 60%;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;


	}

	.text {
		padding-left: 20rpx;
		color: #000;
		font-size: 30rpx;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
