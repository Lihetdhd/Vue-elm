<template>
	<div class="header">
		<div class="contents">
			<div class="avatar">
				<img width="100%" height="100%" v-bind:src="seller.avatar">
			</div>
			<div class="cont">
				<div class="title">
					<span class="brand"><img src="../../../resource/img/brand@2x.png"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					<span>{{seller.description}}/{{seller.delevertTime}}分钟送达</span>
				</div>
				<div class="menne">
					<span class="icon"><img src="../../../resource/img/decrease_1@2x.png"></span>
					<span class="icon2">{{seller.supports[0].description}}</span>
				</div>
			</div>
			<div v-on:click="supportBut" class="support">
				<span class="count">{{seller.supports.length}}个</span>
				<i class="fa fa-angle-right"></i>
			</div>
		</div>
		<div class="bullentin">
			<span class="bullentin-title"></span>
			<span class="bullentin-text">{{seller.bulletin}}</span>
			<i class="fa fa-angle-right tio"></i>		
		</div>
		<div class="background">
			<img width="100%" height="100%" v-bind:src="seller.avatar">
		</div>
		<div v-show="detailShow" class="detail">
			<div class="detail-wrapper clearfix">
				<div class="detail-main">
					<h1 class="name">{{seller.name}}</h1>
					<v-star :size="48" v-bind:score="seller.score"></v-star>
				</div>
				<div class="title">
					<div class="line"></div>
					<div class="text">优惠信息</div>
					<div class="line"></div>
				</div>
				<ul class="supports" >
					<li class="ioc" v-for="(item,index) in seller.supports">
						<span class="iconw" v-bind:class="classMap[index]"></span>
						<span class="icon-title">{{seller.supports[index].description}}</span>
					</li>
				</ul>
				<div class="title notice">
					<div class="line"></div>
					<div class="text">商家公告</div>
					<div class="line"></div>
				</div>
				<div class="intro">
					<span class="intro-title">{{seller.bulletin}}</span>
				</div>
			</div>
			<div class="detail-close" v-on:click="supportBut">
				<i class="fa fa-times"></i>
			</div>
		</div>
	</div>
</template>
<script>
import star from '../star/star.vue'
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return{
				detailShow:false,
				classMap:['decrease','discount','special','invoice','guarantee']
			};
			
		},
		methods:{
			supportBut(){
				this.detailShow = !this.detailShow
			}
		},
		components:{
			'v-star':star
		}
	};
</script>
<style scoped lang="less">
@import url('../../common/css/font-awesome.min.css');
	.header{
		color:#fff;
		background: #000;
		position: relative;
		background: rgba(7,17,27,0.5);
		overflow: hidden;
		.contents{
			width: 100%;
			padding: 24px 12px 18px 24px;
			font-size: 0px;
			position: relative;
			box-sizing: border-box;
			.avatar{
				width: 64px;
				height: 64px;
				display: inline-block;
				vertical-align: top;
				img{
					border-radius: 4px;
				}
			}
			.cont{
				display: inline-block;
				font-size: 14px;
				margin-left: 16px;
				.title{
					margin:2px 0 8px 0;
					.name{
						font-size: 16px;
						color: rgb(255,255,255);
						font-weight: bold;
						line-height: 18px;
						margin-left: 6px;
					}
					.brand{
						vertical-align: top;
						display: inline-block;
					}
					img{
						width: 30px;
						height: 18px;
						background-repeat: no-repeat;
					}
				}
				.description{
					margin-bottom: 6px;
					font-size: 12px;
					font-weight: 200;
				}
				.menne{
					font-size: 10px;
					.icon{
						display: inline-block;
						vertical-align: top;
					}
					img{
						width: 15px;
					}
					.icon2{
						line-height: 16px;
					}

				}
			}
			.support{
				position: absolute;
				right: 12px;
				bottom: 15px;
				padding: 0 8px;
				height:24px;
				font-size: 10px;
				line-height: 24px;
				border-radius:14px;
				background: rgba(0,0,0,0.2);
				text-align: center; 
			}

		}
		.bullentin{
			height: 28px;
			line-height: 28px;
			padding: 0 22px 0 12px;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			font-size: 10px;
			position: relative;
			background: rgba(7,17,27,0.2);
			.bullentin-title{
				margin-top: 8px;
				display: inline-block;
				width: 22px;
				height: 12px;
				background-image: url(../../../resource/img/bulletin@2x.png);
				background-size: 100%;
			}
			.bullentin-text{
				vertical-align: top;
			}
			.tio{
				position: absolute;
				top:8px;
				right: 10px;
			}
		}
		.background{
			width: 100%;
			height: 100%;
			position: absolute; 
			top:0;
			left: 0;
			z-index: -1;
			filter: blur(10px);
		}
		.detail{
			position: fixed;
			top: 0;
			left: 0;
			z-index: 100;
			width: 100%;
			height: 100%; 
			overflow: auto;
			background: rgba(3,13,27,0.8);
			.detail-wrapper{
				min-height: 100%;
				width: 100%;
				transition: all 1s;
				.title{
					display: flex;
					width: 80%;
					margin: 30px auto 24px auto;
					.line{
						flex: 1;
						position: relative;
						top: -6px;
						border-bottom: 1px solid rgba(255,255,255,0.2)
					}
					.text{
						padding: 0 12px;
						font-size: 14px;
					}
				}
				.supports{
					width: 80%;
					margin: 0 auto;
					.ioc{
						font-size:12px;
						color: rgb(255,255,255);
						height: 16px;
						margin-bottom: 12px;
						margin-left: 12px;
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
						}
					}
				}
				.notice{
					margin: 28px auto 24px auto;
				}
				.intro{
					width: 80%;
					margin: 0 auto;
					font-size: 12px; 
					.intro-title{
						display: block;
						padding: 0 12px 0 12px;
						line-height: 1.8em;
					}
				}
			}
			.detail-main{
				margin-top:64px;
				.name{
					line-height: 16px;
					text-align: center;
					font-size:16px;
				}
			}
			.detail-close{
					position: relative;
					width: 32px;
					height: 32px;
					margin:-64px auto 0 auto;
					clear: both;
					font-size:32px;
				}
		}
	}
</style>