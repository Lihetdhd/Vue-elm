<template>
	<div class="seller" ref="Seller">
		<div class="seller-content" >
			<div class="overview">
				<h1 class="title">{{seller.name}}</h1>
				<div class="desc">
					<v-star :size="36":score="seller.score"></v-star> 
					<span class="text">({{seller.ratingCount}})</span>
					<span class="text">月售{{seller.sellCount}}单</span>
				</div>
				<ul class="remark">
					<li class="block">
						<h2>起送价</h2>
						<div class="content">
							<span class="stress">{{seller.minPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>商家配送</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>平均配送时间</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryTime}}</span>元
						</div>
					</li>
				</ul>
				<div class="favorite" @click="togg">
					<i class="fa fa-heart" :class="{'active':favorite}"></i>
					<span>{{favoriteText}}</span>
				</div>
			</div>
			<v-split></v-split>
			<div class="bulletin">
				<h1 class="title">公告与活动</h1>
				<div class="con-wrapper">
					<p class="content">{{seller.bulletin}}</p>
				</div>
				<ul v-if="seller.supports" class="supports">
					<li class="ioc" v-for="(item,index) in seller.supports">
						<span class="iconw" v-bind:class="classMap[index]"></span>
						<span class="icon-title">{{seller.supports[index].description}}</span>
					</li>
				</ul>
			</div>
			<v-split></v-split>
			<div class="pics">
				<h1 class="title">商家实景</h1>
				<div class="pic-wrapper" ref="picwraa">
					<ul class="pic-list" ref="picList">
						<li class="pic-item" v-for="pic in seller.pics">
							<img :src="pic" width="120" height="120">
						</li>
					</ul>
				</div>
			</div>
			<v-split></v-split>
			<div class="info">
				<div class="title">商家信息</div>
				<ul>
					<li class="info-item" v-for="info in seller.infos">{{info}}</li>
				</ul>
			</div>
		</div>
	</div> 
</template>
<script>
import star from '../star/star.vue';
import split from '../split/split';
import BScroll from 'better-scroll';
	export default {
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return{
				favorite:false,
				classMap:['decrease','discount','special','invoice','guarantee']
			}
		},
		created(){
			this.$nextTick(function(){
				console.log(this.seller.pics);
				let picWidth = 120;
				let margin = 6;
				let width = (picWidth + margin) * this.seller.pics.length - margin;
				this.$refs.picList.style.width = width + 'px';
				this.picScroll = new BScroll(this.$refs.picwraa,{scrollX: true});
				this.meunScroll = new BScroll(this.$refs.Seller,{click:true});
			});	
		},
		computed:{
			favoriteText(){
				return this.favorite ? '已收藏':'收藏';
			}
			

		},
		methods:{
			togg(){
				this.favorite = !this.favorite;
			}
		},
		components:{
			'v-split':split,
			'v-star':star
		} 
	}
</script>
<style scoped lang="less">
@import url('../../common/css/font-awesome.min.css');
	.seller{
		position:absolute;
		top:174px;
		bottom:0;
		left:0;
		width:100%;
		overflow:hidden;
		.overview{
			padding: 18px;
			.title{
				margin-bottom:8px;
		      	line-height:14px;
		      	color:rgb(7, 17, 27);
		      	font-size:14px; 	
			}
			.desc{
	      		padding-bottom:18px;
			    font-size:0;
			    .star{
			    	display:inline-block;
			        vertical-align:top;
			        margin:0 8px 0 0;
			    }
			    .text{
			    	display: inline-block;
			    	vertical-align:top;
			        margin-right:12px;
			        line-height:18px;
			        font-size:10px;
			        color:rgb(77, 85, 93);
			    }
		    }
		    .remark{
		    	display:flex;
      			padding-top:18px;
      			.block{
      				flex:1;
			        text-align:center;
			        border-right:1px solid rgba(7, 17, 27, 0.1);
			        &:last-child{
			        	border:none;
			        }
			        h2{
			        	margin-bottom: 4px;
			        	line-height:10px;
			          	font-size:10px;
			         	color:rgb(147, 153, 149);
			        }
			        .content{
			        	line-height:24px;
				        font-size:10px;
				        color:rgb(7, 17, 27);
				        .stress{
				        	font-size: 24px;
				        }
			        }
      			}
		    }
		    .favorite{
		    	position:absolute;
			    right:11px;
			    top:18px;
			    width:50px;
			    text-align:center;
			    .fa-heart{
			    	display:block;
			        margin-bottom:4px;
			        line-height:24px;
			        font-size:24px;
			        width:50px;
			        color:#d4d6d9;
			        &.active{
			        	color:rgb(240,20,20);
			        }
			    }
		    }

		}
		.bulletin{
			padding: 18px 18px 0 18px;
			.title{
				margin-bottom: 8px;
			    line-height :14px;
			    color:rgb(7, 17, 27);
			    font-size :14px;
			}
			.con-wrapper{
				padding:0 12px 16px 1px;
				.content{
					line-height:24px;
        			font-size:12px color rgb(240, 20, 20);
				}
			}
			.supports{	
				.ioc{
					border-top: 1px solid rgba(7, 17, 27, 0.1);
					padding:16px 12px;
			        font-size:0;
					.iconw{
						display: inline-block;
						width: 16px;
						height: 16px;
						margin-right: 6px;
						background-size: 100%;
						vertical-align: top;
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
					.icon-title{
						line-height: 16px;
						font-size:12px;
				        color:rgb(7, 17, 27);
					}
				}
			}
		}
		.pics{
			padding: 18px;
			.title{
				margin-bottom:12px;
			    line-height:14px;
			    color:rgb(7, 17, 27);
			    font-size:14px;
			}
			.pic-wrapper{
				width:100%;
				overflow:hidden;
				white-space: nowrap;
				.pic-list{
					font-size: 0;
					.pic-item{
						display:inline-block;
			            margin-right :6px;
			            width:120px;
			            height:90px;
					}
				}
			}
		}
		.info{
			padding: 18px 18px 0 18px;
			color:rgb(7, 17, 27);
			.title{
				padding-bottom:12px;
		        line-height:14px;
		        font-size:14px;
			}
			.info-item{
				padding:16px 12px;
		      	line-height:16px;
		      	font-size:12px;
			}
		}
	}
</style>