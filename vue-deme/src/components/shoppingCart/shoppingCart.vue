<template>
	<div>
		<div class="shopping_cart">
			<div class="cart_left" @click="toffleList">
				<div class="left_logo">
					<div class="logo_text" :class="{'highlight':totalCount > 0}">
						<i class="fa fa-shopping-cart" :class="{'highlight':totalCount > 0}"></i>
					</div>
					<div class="num" v-show="totalCount > 0">{{totalCount}}</div>
				</div>
				<div class="price">¥{{totalPrice}}</div>
				<div class="dese">另需配送费¥{{seller.deliveryPrice}}元</div>
			</div>
			<div class="cart_right" :class="payClass">
				{{payDesc}}
			</div>
			<div class="ball-contau">
				<div v-for="ball in balls" >
					<transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
						<div v-show="ball.show" class="ball">
							<div class="inner inner-hook" ref="hook"></div>
						</div>
					</transition>
				</div>
			</div>
			<transition name="fade">
				<div class="shopcart-list" v-show="ListShow">
					<div class="list-header">
						<h1 class="title">购物车</h1>
						<span class="empty" @click="empty">清空</span>
					</div>
					<div class="list-cintent" ref="cintert">
						<ul>
							<li class="food" v-for="food in selectFoods">
								<span class="name">{{food.name}}</span>
								<div class="price">
									<span>¥{{food.price*food.count}}</span>
								</div>
								<div class="cartcontr-wrapp">
									<v-cartcont :food="food"></v-cartcont>
								</div>
							</li>
						</ul>
					</div>
				</div>
			</transition>
			
		</div>
		<transition name="fade">
			<div class="List-mask" v-show="ListShow" @click="toffleList"></div>
		</transition>
	</div>
</template>
<script type="text/javascript">
	import cartcontrol from '../cartcontrol/cartcontrol';
	import BScroll from 'better-scroll';
	export default{
		props:{
			seller:{
				type:Object
			},
			selectFoods:{
				type:Array,
				difault(){
					return [{price:20,count:2}];
				}
			}
		},
		data(){
			return{
				balls:[{show:false},{show:false},{show:false},{show:false},{show:false}],
				dropBalls:[],
				fold:true
			}
		},
		components:{
			'v-cartcont':cartcontrol
		},
		computed:{
			totalPrice(){
				let total = 0;
				this.selectFoods.forEach(function(food){
					total += food.price * food.count;
				});
				return total;
			},
			totalCount(){
				let count = 0;
				this.selectFoods.forEach(function(food){
					count += food.count;
				});
				return count;
			},
			payDesc(){
				if(this.totalPrice === 0){
					return `¥元${this.seller.minPrice}起送`;
				}else if(this.totalPrice < this.seller.minPrice){
					let dif = this.seller.minPrice - this.totalPrice;
					return `还差¥${dif}元起送`;
				}else{
					return '去结算';
				}
			},
			payClass(){
				if(this.totalPrice < this.seller.minPrice){
					return 'not-enough';
				}else{
					return 'enough';
				}
			},
			ListShow(){
				if(!this.totalCount){
					this.fold = true;
					return false;
				}
				let show = !this.fold;
				if(show){
					this.$nextTick(function(){
						if(!this.scroll){
							this.scroll = new BScroll(this.$refs.cintert,{click:true});
						}else{
							this.scroll.refresh();
						}
					});
				}
				return show;
			}
		},
		methods:{
			drop(el){
				for(let i=0;i<this.balls.length;i++){
					let ball = this.balls[i];
					if(!ball.show){
						ball.show = true;
						ball.el = el;
						this.dropBalls.push(ball);
						return; 
					}
				}
			},
			beforeEnter(el){
				let count = this.balls.length;
				while(count--){
					let ball = this.balls[count];
					if(ball.show){
						let rect = ball.el.getBoundingClientRect();
						let x = rect.left - 32;
						let y = -(window.innerHeight - rect.top - 22);
						el.style.display = '';
						el.style.webkitTransform = `translate3d(0,${y}px,0)`;
						el.style.transform = `translate3d(0,${y}px,0)`;
						//let inner = this.$refs.hook[0];
						let inner = el.getElementsByClassName('inner-hook')[0];
						inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
						inner.style.transform = `translate3d(${x}px,0,0`;
					}
				}
			},
			enter(el){
				let rf = el.offsetHeight;
				this.$nextTick(function(){
					el.style.webkitTransform = 'translate3d(0,0,0)';
					el.style.transform = 'translate3d(0,0,0)';
					// let inner = this.$refs.hook[0];
					let inner = el.getElementsByClassName('inner-hook')[0];
					inner.style.webkitTransform = 'translate3d(0,0,0)';
					inner.style.transform = 'translate3d(0,0,0)';
				});
					 
			},
			afterEnter(el){
				let ball = this.dropBalls.shift();
					if(ball){
					ball.show = false;
					el.style.display = 'none';
					}
			},
			toffleList(){
				if(!this.totalCount){
					return;
				}
				this.fold = !this.fold;
			},
			empty(){
				this.selectFoods.forEach(function(food){
					food.count = 0;
				});
			}
		}
	}
</script>
<style scoped lang="less">
@import url('../../common/css/font-awesome.min.css');
	.shopping_cart{
		position: fixed;
		display: flex;
		bottom: 0px;
		left: 0px;
		z-index: 9;
		font-weight: 700;
		width: 100%;
		height: 48px;
		background: #141d27;
		color: rgba(255,255,255,0.4);
		.cart_left{
			position: relative;
			height: 48px;
			flex: 1;
			.left_logo{
				margin:-10px 12px; 
				position: relative;
				background: #141d27;
				border-radius: 50%;
				box-sizing: border-box;
				display: inline-block;
				vertical-align: top;
				width: 56px;
				height: 56px;
				padding: 6px;
				.logo_text{
					width: 100%;
					height:100%;
					border-radius: 50%;
					background:#2b343c; 
					text-align: center;
					&.highlight{
						background: rgb(0,160,220);
						color:#fff;
					}
					i{
						margin-right: 4px;
						line-height: 44px;
					    font-size: 24px;
					    color: #80858a;
					    &.highlight{
					    	color: #fff;
					    }   
					} 
				}
				.num{
					position: absolute;
					top:0;
					right: 0;
					width: 24px;
					height: 16px;
					text-align: center;
					line-height: 16px;
					border-radius:16px;
					font-size: 9px;
					color: #fff;
					background: red;
					box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
				}
			}
			.price{
				display: inline-block;
				font-weight: 700;
				height: 24px;
				font-size: 20px;
				padding-right:12px; 
				line-height: 24px;
				margin-top:12px;
				margin-left:6px;  
				padding-right: 12px;
				vertical-align: top;
				border-right: 1px solid rgba(255,255,255,0.1);
			}
			.dese{
				display: inline-block;
				height: 24px;
				font-size: 10px;
				margin-top:12px; 
				line-height: 24px;
				margin-left:12px; 
			}
		}
		.cart_right{
			flex: 0 0 105px;
			height: 48px;
			line-height: 48px;
			text-align: center;
			font-size:12px;
			background: #2b333b;
			color: rgba(255,255,255,0.4);
			&.not-enough{
				background:#2b333b;
			}
			&.enough{
				background: #00b43c;
				color: #fff;
			} 
		}
		.ball-contau{
			.ball{
				position: fixed;
				left: 32px;
				bottom: 22px;
				z-index: 200;
				transition: all 0.6s cubic-bezier(0.49,-0.29,0.75,0.41);
				.inner{
						width: 16px;
						height: 16px;
						border-radius: 50%;
						background: rgb(0,160,220);
						transition: all 0.4s linear;
					}
			}
		}
		 .shopcart-list{
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			z-index: -5;
			transform :translate3d(0, -100%, 0);
			 &.fade-enter-active, &.fade-leave-active {	
		      transition: all 0.5s;
		      transform:translate3d(0, -100%, 0);
		    }
		    &.fade-enter, &.fade-leave-active {
		      transform :translate3d(0, 0, 0);
		    }
		    .list-header{
		    	font-size:14px;
		    	height: 40px;
		    	line-height: 40px;
		    	padding: 0 18px;
		    	background: #f3f5f7;
		    	border-bottom: 1px solid rgba(7,17,27,0.1);
		    	.title{
		    		color: rgb(7,17,27);
		    		float: left;
		    	}
		    	.empty{
		    		float: right;
		    		font-size: 12px;
		    		color: rgb(0,160,220);
		    	} 
		    }
		    .list-cintent{
		    	padding: 0 18px;
		    	max-height: 217px;
		    	background: #fff;
		    	overflow: hidden;
		    	.food{
		    		height: 48px;
		    		padding: 12px 0;
		    		position: relative;
		    		box-sizing: border-box;
		    		border-bottom: 1px solid rgba(7,17,27,0.1);
		    		.name{
		    			line-height: 24px;
		    			font-size: 14px;
		    			color: rgb(7,17,27);
		    		}
		    		.price{
		    			position: absolute;
		    			bottom: 12px;
		    			right: 90px;
		    			line-height: 24px;
		    			font-size: 14px;
		    			font-weight: 700;
		    			color:red;	
		    		}
		    		.cartcontr-wrapp{
		    			position: absolute;
		    			right: 0;
		    			bottom: 6px;
		    		}

		    	}
		    }

		}	
	}
	.List-mask{
			position: fixed;
			top:0;
			left: 0;
			width: 100%;
			height: 100%;
			z-index:8;
			opacity: 1;
			background:rgba(7, 17, 27, 0.6);
			&.fade-enter-active, &.fade-leave-active{
				opacity: 1;
				transition: all 0.5s;
				background: rgba(7,17,27,0.6);
			}
			&.fade-enter, &.fade-leave-active{
				opacity: 0;
				background: rgba(7,17,27,0);
			} 
		}
	 
</style>