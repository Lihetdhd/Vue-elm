<template>
	<div class="goods">
		<div class="menu">
			<ul>
				<li v-for="(teim,index) in goods" class="menu-classify" v-bind:class="{'current':currentIndex === index}" v-on:click="selectMenu(index,$event)">
					<span class="iconw" v-bind:class="classMap[index]" v-show="classMap[index]"></span>
					<span class="sidf-txt">{{teim.name}}</span>
				</li>
			</ul>
		</div>
		<div class="wapper">
			<ul>
				<li v-for="item in goods" class="food-list food-list-hook" ref="foodlist">
					<div class="title">
						<span class="title-text">{{item.name}}</span>
					</div>
					<ul>
						<li @click="selectfood(food,$event)" v-for="food in item.foods" class="food-item">
							<div class="img">
								<img :src="food.icon">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span>月售{{food.sellCount}}份</span>
									<span class="yuep">好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span>
									<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
								</div>
								<div class="cart-wrapper">
									<v-cartcont :food="food" @increment="increementtal"></v-cartcont>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<v-shopping :seller="seller" :selectFoods="selectFoods" ref="shopCart"></v-shopping>
		<v-foods :food="selectedFood" ref="food" @increment="increementtal"></v-foods>
	</div>
</template>
<script>
	import BScroll from 'better-scroll';
	import shopping from '../shoppingCart/shoppingCart.vue';
	import cartcontrol from '../cartcontrol/cartcontrol';
	import foods from '../food/food';
	export default {
		name:"Goods",
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return{
				goods:[],
				classMap:['','special'],
				listHeight:[],
				scrollY:0,
				selectedFood:{}
			}
		},
		components:{
			'v-shopping':shopping,
			'v-cartcont':cartcontrol,
			'v-foods':foods
		},
		beforeMount(){
			// this.goods = this.$route.params.goods;
			// console.log(this.$route.params);
		},
		created(){
		  this.$http.get('http://localhost:8080/static/data.json').then(function res(e){
	          //this.seller = e.body.seller;
	          this.goods = e.body.goods;
	          // console.log(e.body.goods)
	          //this.ratings = e.body.ratings;
	          	this.$nextTick(()=>{
					this.caleuHeight();
					this.initScroll(); 
				});			
           });
		},
		computed:{
			currentIndex(){
				for(let i=0;i<this.listHeight.length;i++){
					let height1 = this.listHeight[i];
					let height2 = this.listHeight[i+1];
					if(!height2||(this.scrollY>=height1 && this.scrollY<height2)){
						return i;
					}
				}
				return 0;
			},
			selectFoods(){
				let foods = [];
				this.goods.forEach(function(good){
					good.foods.forEach(function(food){
						if(food.count){
							foods.push(food);
						}
					});
				});
				return foods;
			}
		},
		methods:{
			initScroll(){
				let that = this;
				this.meunScroll = new BScroll('.menu',{click:true});
				this.foodsScroll = new BScroll('.wapper',{click:true,probeType:3});
				this.foodsScroll.on('scroll',function(e){
						that.scrollY = Math.abs(Math.round(e.y));
				});
			},
			selectMenu(e,event){
				/*if(!event._constructed){
					// 去掉自带click事件的点击
					return;
				}*/
				let foodList = this.$refs.foodlist;
				let el = foodList[e];
				this.foodsScroll.scrollToElement(el,300);
				
			},
			caleuHeight(){
				//另一种获取dom元素的方法
				/*let foodList = document.getElementsByClassName("food-list-hook");
				// let foodList = document.querySelectorAll(".food-list-hook");
				let height = 0;
				this.listHeight.push(height);
				let obj=[];
				for(let i in foodList){
					console.log(i);
					if(foodList[i].clientHeight){
						obj.push(foodList[i].clientHeight);
					}
				}
				console.log(obj);*/
				let foodList = this.$refs.foodlist;
				let height = 0;
				this.listHeight.push(0);
				for(let i=0;i<foodList.length;i++){
					height += foodList[i].clientHeight;
					this.listHeight.push(height);
				}
			},
			increementtal(target){
				this.$nextTick(function() {
         			this.$refs.shopCart.drop(target);
       			});
			},
			selectfood(food,e){
				this.selectedFood = food;
				this.$refs.food.show();
			}
		}
	}
</script>
<style scoped lang="less">
	.goods{
		display: flex;
		position: absolute;
		top:174px;
		bottom: 46px;
		width: 100%;
		overflow: hidden;
		.menu{
			flex: 0 0 80px;
			width:80px;
			background: #f3f5f7;
			.menu-classify{
				width:56px;
				height: 54px;
				padding: 0 12px;
				line-height: 14px;
				text-align: center;
				display: table;
				&.current{
        			font-weight:700;
					background: #fff;
				}
				.iconw{
					display: inline-block;
					width: 16px;
					height: 16px;
					margin: 20px 2px 0 0;
					background-size: 100%;
					&.decrease{
						background-image: url(../../../resource/img/decrease_1@2x.png);
					}
					&.discount{
						background-image: url(../../../resource/img/discount_1@2x.png);
					}
					&.special{
						background-image: url(../../../resource/img/special_1@2x.png);
					}
					&.invoice{
						background-image: url(../../../resource/img/invoice_1@2x.png);
					}
					&.guarantee{
						background-image: url(../../../resource/img/guarantee_1@2x.png);
					}
				}
				.sidf-txt{
					font-size: 12px;
					display: table-cell;
					vertical-align: middle;
				}
			}
		}
		.wapper{
			flex:1;
			.food-list{
				
				.title{
					height: 26px;
					background: #f3f5f7;
					border-left: 3px solid #d9dde1;
					.title-text{
						font-size: 12px;
						color: rgb(147,153,159);
						line-height: 26px;
						margin-left: 14px;
					}
				}
				ul{
					.food-item{
						display: flex;
						margin:18px;
						padding-bottom:18px; 
						border-bottom: 1px solid rgba(7,17,27,0.1);
						.img{
							flex: 0 0 57px;
							margin-right:10px; 
							img{
								width:57px;
							}
						}
						.content{
							flex: 1;
							position: relative;
							.name{
								font-size: 14px;
								color:rgb(7,17,27);
								margin-top: 2px;
								height: 14px;
								line-height: 14px;
							}
							.desc{
								margin: 8px 0 8px 0;
								font-size: 10px;
								color:rgb(147,153,159);
							}
							.extra{
								font-size: 10px;
								color:rgb(147,153,159);
								.yuep{
									margin-left:12px; 
								}
							}
							.price{
								font-weight: 700;
								margin-top:8px; 
								.now{
									 margin-right: 8px;
									 font-size: 14px;
									 color: rgb(240,20,20);
								}
								.old{
									font-size: 10px;
									color: rgb(147,153,159);
									text-decoration: line-through;
								}
							}
							.cart-wrapper{
								position: absolute;
								right: 0;
								bottom:-10px;
							}
						}
					}
				}
			}
		}
	}
</style>