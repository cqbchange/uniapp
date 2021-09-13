<template>
	<view class="shoppmain">
		<view class="shopptitle shoppstyle">
			<view class="address">
				<text class="iconfont icon-dizhi"></text>
				<text>{{address}}</text>
			</view>
			<view class="line">

			</view>
			<view class="bianji" @click="compile">
				<text v-if="bianji">编辑</text> 
				<text v-else>完成</text> 
			</view>
			<view class="icon">
				<text class="iconfont iconfs icon-shenglvehao"></text>
			</view>
		</view>

		<view class="shoppingselect">
			<view class="">
				京东自营店
			</view>
			<checkbox-group>
				<view class="shopptitle shopplabel" v-for="(item,index) in shoppingdata" :key="index">
					

				<label >
					<view>
						<checkbox class="radio"   :checked="(item.checked)" @click="select(item,index)" />
					</view>
					</label>
					<view class="">
						<image :src="(item.img)" class="shoppimg"></image>
					</view>		  
					<view class="shoptext">
						<view class="shoppheade"> <text class="headetext">精品手机</text> {{item.good_name.name}}</view>
						<view class="shoppsku">{{item.sku}}</view>
						<view class="shopptitle">
							<view class="shopptitle shopppric">
							 <text class="shopppri">￥</text> <text>{{item.price}} </text>
							</view>
							<view class="shopptitle">
								<view class="shoppsub" @click="sub(item,index)">
									<text class="iconfont shoppsub icon-jian"></text>
								</view> 
								<input class="uni-input" type="number" @input="onKeyInput":id='index' :value="(item.num)" />
								<view class="shoppsub" @click="sup(item,index)">
									<text class="iconfont shoppsub icon-jia"></text>
								</view>
							</view>
						</view>
					</view>
				</view>
			</checkbox-group>
		</view>
		<view class="shopptitle shoppstyle shoppbottom">
			<view class="address icon">
				<checkbox class="radio"  :checked="checked" @click="allcheck"/>
				<text>全选</text>
			</view>
			<view class="shopptitle" v-if="bianji">	
			<view class="bianji icon">
				总价:{{shoppprice}}
			</view>
			<view class="final  icon" @click="final">
				去结算[{{shoppnum}}]
			</view>
			</view>
			<view v-else class=" final " @click="deletes">
				删除
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				address: "四川省武侯区孵化园9号楼e座",
				shoppingdata: [],
				checked: true,
				nums:'',
				good_id:'',
				total:'',
				sku:'',
				skuspri:'',
			    shopping_car_id:'',
				shoppprice:0,
				shoppnum:0,
				bianji:true
			}
		},
		methods: {
			//结算
			
			final(){
				
				let obj={}
				let arr=[]
				let arrcarids=[]
				this.shoppingdata.forEach(item=>{
					if(item.checked==true){
						arr.push(item)
						arrcarids.push(item.shopping_car_id)
					}
				})
				obj.arr=arr
				obj.arrcarids=arrcarids
				obj.shoppprice=this.shoppprice
			 let a= JSON.stringify(obj)
			 if(arr){
				 
			 uni.navigateTo({
			 	url: '/pages/account/account?a=' + a
			 })
			 }
			},
			//删除shopping_car_id api/shoppingCarDelete
			deletes(){
				let arr=[]
				let s= new Set(this.shoppingdata)
				this.shoppingdata.forEach((item,index )=>{
					if(item.checked==true){
						arr.push(item.shopping_car_id)
						 s.delete(item)
					}
				})
				this.shoppprice=0,
				this.shoppnum=0,
				 this.shoppingdata = Array.from(s)
				 if(this.shoppingdata.length==0){
				 	this.checked=false
				 }
				console.log(arr)
				for(let i=0;i<arr.length;i++){
					uni.request({
						url: 'http://api_devs.wanxikeji.cn/api/shoppingCarDelete',
						data: {
							token: uni.getStorageSync("token"),
							shopping_car_id:arr[i]
						},
						success(res) {
							console.log(res)
							uni.showToast({
								title: res.data.msg,
								duration: 2000
							});
						}
					})
				}
			},
			//编辑
			compile (){
				if(this.bianji==true){
				this.bianji=false
				}else{
					this.bianji=true
				}
			},
			//全选
			allcheck(){
				if(this.checked==true){
				this.checked=false
				}else{
					this.checked=true
				}
				let p=0
					this.shoppingdata.forEach(item=>{
						item.checked=this.checked
						if(this.checked==true){
							this.shoppnum+=item.num
							p+=item.num*item.price
						  
						}else{
							this.shoppprice=0
							this.shoppnum=0
						} 
					})
				 this.shoppprice=p.toFixed(2)
			},
			
			amend(){
				let  than=this
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
				}
			})
			},
			//点选
			select(item,index){
				if(	item.checked==true){
					item.checked=false
					// this.checked=false
				}else{
					item.checked=true
				}
				let n=0
				let p=0
				let arr=[]
				this.shoppingdata.forEach((item,index )=>{
					if(item.checked==true){
						arr.push(index)
						n+=item.num
						p+=item.num*item.price
					}
				})
				this.shoppnum=n
				this.shoppprice=p.toFixed(2)
				if(arr.length==this.shoppingdata.length){
					this.checked=true
				}else{
					this.checked=false
				}
			},
			onKeyInput(e) {
				console.log(e)
				let num = e.target.value
				let i=e.currentTarget.id
				let item=this.shoppingdata[i]
				console.log(this.shoppingdata[i],num)
				if(num-0>0){
					item.checked=true
					this.shoppingdata[i].num=Number(num)
					let arr=[]
					let n=0
					let p=0
					this.shoppingdata.forEach((item,index )=>{
						if(item.checked==true){
							arr.push(index)
							n+=item.num
							p+=item.num*item.price
						}
					})
					this.shoppnum=n
					this.shoppprice=p.toFixed(2)
					if(arr.length==this.shoppingdata.length){
						this.checked=true
					}else{
						this.checked=false
					}
					
				this.good_id=item.good_id//商品id
				this.total=item.num*item.price//单个总价
				this.nums=item.num//数量
				this.skuspri=item.price//单价
				this.shopping_car_id=item.shopping_car_id//购物车
				this.sku=item.sku//sku
				this.amend()
				}else{
					item.num=1
					let n=0
					let p=0
						let arr=[]
					this.shoppingdata.forEach((item,index )=>{
						if(item.checked==true){
							n+=item.num
							arr.push(index)
							p+=item.num*item.price
						}
					})
					if(arr.length==this.shoppingdata.length){
						this.checked=true
					}else{
						this.checked=false
					}
					this.shoppnum=n
					this.shoppprice=p.toFixed(2)
					this.good_id=item.good_id//商品id
					this.total=item.num*item.price//单个总价
					this.nums=item.num//数量
					this.skuspri=item.price//单价
					this.shopping_car_id=item.shopping_car_id//购物车
					this.sku=item.sku//sku
					this.amend()
				}
			},
			sub(item,index) {
				item.num--
				if (item.num < 1) {
					item.num = 1
					return
				}
				item.checked=true
				let arr=[]
				let n=0
				let p=0
				this.shoppingdata.forEach((item,index )=>{
					if(item.checked==true){
						arr.push(index)
						n+=item.num
						p+=item.num*item.price
					}
				})
				this.shoppnum=n
				this.shoppprice=p.toFixed(2)
				if(arr.length==this.shoppingdata.length){
					this.checked=true
				}else{
					this.checked=false
				}
				this.good_id=item.good_id//商品id
				this.total=item.num*item.price//单个总价
				this.nums=item.num//数量
				this.skuspri=item.price//单价
				this.shopping_car_id=item.shopping_car_id//购物车
				this.sku=item.sku//sku
				this.amend()
			},
			sup(item,index) {	
				item.num++
				if (item.num> 200) {
					item.num = 200
				}
				// console.log(item)
				 item.checked=true
				 let arr=[]
				 let n=0
				 let p=0
				 this.shoppingdata.forEach((item,index )=>{
				 	if(item.checked==true){
				 		arr.push(index)
				 		n+=item.num
				 		p+=item.num*item.price
				 	}
				 })
				 this.shoppnum=n
				 this.shoppprice=p.toFixed(2)
				 if(arr.length==this.shoppingdata.length){
				 	this.checked=true
				 }else{
				 	this.checked=false
				 }
				 this.good_id=item.good_id//商品id
				 this.total=item.num*item.price//单个总价
				 this.nums=item.num//数量
				 this.skuspri=item.price//单价
				 this.shopping_car_id=item.shopping_car_id//购物车
				 this.sku=item.sku//sku
				 this.amend()
				// if(item.checked==true){
				//   this.shoppnum
				//   this.shoppprice= Number(this.shoppprice)+Number(item.price)
				// }
			},
			request() {
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
				let than = this
				uni.request({
					url: 'http://api_devs.wanxikeji.cn/api/shoppingCarList',
					data: {
						token: uni.getStorageSync("token")
					},
					success(res) {
						console.log(res)

						let list = res.data.data
						let  n=0
						let  p=0
						// for (let i = 0; i < list.length; i++) {
							console.log(list)
							if(list.length!==0){
							list.forEach((item,index)=>{
							if (item.good_name.indexOf("lsycqb") >= 0) {
								item.good_name = JSON.parse(item.good_name)
								item.checked = true
								n+=item.num
								p+=( (item.num)*(item.price-0))
							}
							}
							)
							}else{
								
							}
						// }
						than.shoppnum=n
						than.shoppprice=p.toFixed(2)
						than.shoppingdata = list
						than.shoppingcarlen = res.data.data.count
						if(list.length==0){
							than.checked=false
						}
					}
				})
			},
		},
		// beforeCreate(){
		// 	this.request()
		// },
		onLoad(opctions) {
			this.request()
			// uni.startPullDownRefresh();
		},
		onShow(){
			this.request()
		},
		 onPullDownRefresh() {
		        console.log('refresh');
		        // setTimeout(function () {
		        //     uni.stopPullDownRefresh();
		        // }, 1000);
		    }
	}
</script>

<style>
	.shoppmain {
		background-color: #F4F4F4;
		margin-bottom: 120rpx;
	}

	.shopptitle {
		display: flex;
		background-color: #FFFFFF;
	}

	.shoppstyle {
		padding: 20rpx;
	}

	.address {
		flex-grow: 1;
	}

	.line {
		width: 1rpx;
		background-color: #8F8F94;
	}

	.bianji {
		margin: 0 20rpx;
	}

	.icon {
		justify: center;
		align-items: center;
		display: -webkit-flex;
	}
	.iconfs{
		font-size:20rpx;
	}
.shopppri{
	font-size: 20rpx;
}
	.shoppingselect {
		margin: 20rpx 0;
		padding: 20rpx;
		background-color: #FFFFFF;
	}

	.shoppimg {
		width: 200rpx;
		height: 200rpx;
	}

	.shopplabel{
		padding: 20rpx 0;
		border-bottom: 1rpx solid rgba(80,80,80,.5);
	}
	.uni-input {
		width: 60rpx;
		margin: 0 20rpx;
		background-color: #C8C7CC;
		border-radius: 5rpx;
	}
	.shoptext{
		margin-left: 20rpx;
		flex-grow: 1;
	}
	.shoppheade{
		    overflow: hidden;
		    text-overflow: ellipsis;
		    display: -webkit-box;
		    -webkit-box-orient: vertical;
		    -webkit-line-clamp: 2;
	}
	.headetext{ 
		background-color: #FF4142;
		color: #FFFFFF;
		padding: 0 10rpx;
		margin-right:10rpx;
	}
	.shoppsku{
		  padding:  20rpx 0;
	}
	.shoppsub{
       color: #515151;
		font-size: 30rpx;
	}
	.shopppric{
		flex-grow: 1;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 1;
		color: #FF4142 ;
	}
	.final{
		border-radius: 44rpx;
		background-color: #FF4142;
		padding: 20rpx;
	}
	.shoppbottom{
		position: fixed;
        bottom: 0;
		width: 100%;
		z-index: 99;
		box-sizing: border-box;
	} 


</style>
