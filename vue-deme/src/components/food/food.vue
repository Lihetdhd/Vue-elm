<template>
	<transition name="fade">
		<div v-show="showFlag" class="food" ref="Food">
			<div class="food-content">
				<div class="image-header">
					<img :src="food.image">
					<div class="back" @click="show">
						<i class="fa fa-angle-left"></i>
					</div>
				</div>
				<div class="content">
					<h1 class="title">{{food.name}}</h1>
					<div class="detail">
						<span class="sell-count">月售{{food.sellCount}}份</span>
						<span class="rating">好评{{food.rating}}%</span>
					</div>
					<div class="price">
						<span class="now">￥{{food.price}}</span>
						<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
					</div>
					<div class="cartcont">
						<v-cart :food="food" @increment="tache"></v-cart>
					</div>
					<transition name="buy">
						<div @click="addfirst($event)" class="buy" v-show="!food.count || food.count===0">加入购物车</div>
					</transition>
				</div>
				<v-split v-show="food.info"></v-split>
				<div class="info" v-show="food.info">
					<h1 class="title">商品信息</h1>
					<p class="text">{{food.info}}</p>
				</div>
				<v-split v-show="food.info"></v-split>
				<div class="rating">
					<h1 class="title">商品评价</h1>
					<v-rating @select-tyle="SelectType" @only-Content="OnlyContent" :selectTypes="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"></v-rating>
					<div class="rating-wrapper">
						<ul v-show="food.ratings && food.ratings.length">
							<li v-show="needshow(rating.rateType,rating.text)" class="rating-item" v-for="rating in food.ratings">
								<div class="user">
									<span class="name">{{rating.username}}</span>
									<img class="avatar" width="12" height="12" :src="rating.avatar"></img>
								</div>
								<div class="time">{{rating.rateTime | formatDate}}</div>
								<p class="text">
									<i class="iconfont" :class="{'fa fa-thumbs-up':rating.rateType===0,'fa fa-thumbs-down':rating.rateType===1}"></i>
									{{rating.text}}
								</p>
							</li>
						</ul>
						<div class="no-rating" v-show="!food.ratings||food.ratings.length===0">暂无评价</div>
					</div>
				</div>
			</div>
		</div>
	</transition>
</template>
<script>
	import BScroll from 'better-scroll';
	import Vue from 'vue';
	import split from '../split/split';
	import ratingselect from '../tatingselect/tatingselect'
	import cartcontrol from '../cartcontrol/cartcontrol';
	import {formatDate} from '../../common/js/data.js';
	const ALL = 2;
	export default{
		props:{
			food:{
				type:Object
			}
		},
		data(){
			return{
				showFlag:false,
				selectType:ALL,
				onlyContent:true,
				desc:{
					all: '全部',
	         		positive: '推荐',
	          		negative: '吐槽'
				}
			};
		},
		components:{
			'v-cart':cartcontrol,
			'v-split':split,
			'v-rating':ratingselect
		},
		methods:{
			show(){
        		this.selectType = ALL;
        		this.onlyContent = true;
				this.showFlag = !this.showFlag;
				this.$nextTick(function(){
					this.meunScroll = new BScroll(this.$refs.Food,{click:true});
				});	
			},
			SelectType(type){
				this.selectType = type;
				this.$nextTick(() => {
		          this.meunScroll.refresh();
		        });
			},
			OnlyContent(con){
				this.onlyContent = con;
				this.$nextTick(() => {
		          this.meunScroll.refresh();
		        });
			},
			addfirst(event){
				Vue.set(this.food,'count',1);
				this.$emit('increment',event.target);
			},
			tache(){
				this.$emit('increment',event.target);
			},
			needshow(type,text){
				if(this.onlyContent && !text){
					return false;
				}
				if(this.selectType === ALL){
					return true;
				}else{
					return type === this.selectType;
				}
			}
		},
		filters:{ 
			formatDate(time){
				let date = new Date(time);
				return formatDate(date,'yyyy-MM-dd hh:mm');
			}
		}
	}
</script>
<style scoped lang="less">
@import url('../../common/css/font-awesome.min.css');
	.food{
		position: fixed;
		left: 0;
		top: 0;
		bottom: 48px;
		z-index: 8;
		width: 100%;
		background: #fff;
		&.fade-enter-active, &.fade-leave-active{
			transition: all 0.2s linear;
			transform: translate3d(0,0,0);
		}
		&.fade-enter, &.fade-leave-active{
			opacity:0;
			transform: translate3d(100%,0,0);
		}
		.image-header{
			position: relative;
			width: 100%;
			height: 0;
			padding-top:100%;
			img{
				position: absolute;
				top:0;
				left: 0;
				width:100%;
				height: 100%;
			}
			.back{
				position: absolute;
				top: 10px;
				left:0;
				.fa-angle-left{
					display: block;
					padding:10px;
					font-size:20px;
					color: #fff;
				}
			} 
		}
		.content{
			padding:18px;
			position: relative;
			.title{
				line-height:14px;
				margin-bottom: 8px;
				font-size:14px;
				font-weight:700;
				color:rgb(7,17,27);
			}
			.detail{
				margin-bottom:18px;
				line-height:10px;
				font-size:0;
				height:10px;
				.sell-count, .rating{
					font-size:10px;
					color:rgb(147,153,159);
				}
				.sell-count{
					margin-right:12px;
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

		}
		.cartcont{
			position: absolute;
			right: 12px;
			bottom: 12px;
		}
		.buy{
			position: absolute;
			right: 18px;
			bottom: 18px;
			z-index: 10;
			height: 24px;
			line-height: 24px;
			padding: 0 12px;
			box-sizing: border-box;
			border-radius: 12px;
			font-size: 10px;
			color: #fff;
			background: rgb(0,160,220);
			&.buy-enter-active, &.buy-leave-active{
				transition: all 0.2s;
				opacity: 0;
			}
			&.buy-enter, &.buy-leave-active{
				opacity: 0;
			}
		}
		.info{
			padding:18px;
			.title{
				line-height:14px;
				margin-bottom:6px;
				font-size:14px;
				color:rgb(7,17,27);
			}
			.text{
				line-height:24px;
				padding:0 8px;
				font-size:12px;
				color:rgb(77,85,93);
			}
		}
		.rating{
			padding-top:18px;
			.title{
				line-height: 14px;
				margin-left: 18px;
				font-size: 14px;
				color: rgb(7,17,27);
			}
			.rating-wrapper{
				padding:0 18px;
				.rating-item{
					position: relative;
					padding: 16px 0;
					border-bottom: 1px solid rgba(1,17,27,0.1);
					.user{
						position: absolute;
						right: 0;
						top:16px;
						font-size: 0;
						line-height: 12px;
						.name{
							display: inline-block;
							vertical-align: top;
							margin-right:6px;
							font-size:10px;
							color: rgb(147,153,159);
							.avatar{
								border-radius:50%;
							}
						}
					}
					.time{
						margin-bottom:6px;
			          	line-height:12px;
			          	font-size:10px;
			          	color:rgb(147, 153, 159);
					}
					.text{
						line-height:16px;
				        font-size:12px;
				        color:rgb(7, 17, 27);
				        .iconfont{
				        	margin-right:4px;
				            line-height:16px;
				            font-size:12px;
				        }
				        .fa-thumbs-up{
				        	color: rgb(0,160,220);
				        }
				        .fa-thumbs-down{
				        	color: rgb(147,153,159);
				        }
					}
				}
				.no-rating{
					padding: 16px 0;
					font-size: 12px;
					color:rgb(147,153,159);
				}
			} 
		}

	}
</style>