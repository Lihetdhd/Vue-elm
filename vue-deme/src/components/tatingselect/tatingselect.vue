<template>
	<div class="ratingselect">
		<div class="rating-type">
			<span class="block positive" @click="select(2,$event)" :class="{'active':Stype===2}">
				{{desc.all}}
				<span class="count">{{ratings.length}}</span>
			</span>
			<span class="block positive" @click="select(0,$event)" :class="{'active':Stype===0}">
				{{desc.positive}}
				<span class="count">{{positives.length}}</span>
			</span>
			<span class="block negative" @click="select(1,$event)" :class="{'active':Stype===1}">
				{{desc.negative}}
				<span class="count">{{nagatives.length}}</span>
			</span>
		</div>
		<div class="switch" :class="{'on':oCintent}" @click="toggleContent">
			<span class="fa fa-check-circle"></span>
			<span class="text">只看有内容的评论</span>
		</div>
	</div>
</template>
<script>
	const POSITIVE = 0;
	const NEGATIVE = 1;
	const ALL = 0;
	export default{
		props:{
			ratings:{
				type:Array,
				default(){
					return[];
				}
			},
			 selectTypes: {
	        	type: Number,
	        	default: ALL
	      	},
			onlyContent:{
				type:Boolean,
				default:false
			},
			desc:{
				type:Object,
				default(){
					return{
						all:'全部',
						positive:'满意',
						negative:'吐槽'
					};
				}
			}
		},
		data(){
			return{ 
				Stype:this.selectTypes,
				oCintent:this.onlyContent
			}
		},
		computed:{
			tyle(){
				this.Stype = this.selectTypes;
			},
			positives(){
				return this.ratings.filter(function(rating){
					return rating.rateType === POSITIVE;
				});
			},
			 nagatives() {
		        return this.ratings.filter(function(rating){
		          return rating.rateType === NEGATIVE;
		        });
     		 }
		},
		methods:{
			select(type){
				this.Stype = type;
				this.$emit('select-tyle',type);
			},
			toggleContent(){
				this.oCintent = !this.oCintent;
				this.$emit('only-Content', this.oCintent);
			}
		}
	}
</script>
<style scoped lang="less">
	@import url('../../common/css/font-awesome.min.css');
	.ratingselect{
		.rating-type{
			padding:18px 0;
			margin: 0 18px;
			font-size:0;
			.block{
				display: inline-block;
				padding: 8px 12px;
				margin-right: 8px;
				border-radius: 1px;
				font-size:12px;
				color: rgb(77,85,93);
				&.positive{
					background :rgba(0, 160, 220, 0.2);
					 &.active{
					 	color: #ffffff;
         				background: rgb(0, 160, 220);
					}
				}
				&.negative{
					background: rgba(77, 85, 93, 0.2);
					&.active{
						color: #ffffff;
          				background:rgb(77, 85, 93);
					}
				}
				.count{
					font-size:8px;
			        margin-left:2px;
			        line-height:16px;
				}

			}
		}
		.switch{
			padding:12px 18px;
		    line-height:24px;
		    font-size:12px;
		    color:rgb(147, 153, 159);
		    border:1px solid rgba(1, 17, 27, 0.1);
		    font-size:0;
		    &.on{
		    	.fa-check-circle{
		    		color: #00c850; 
		    	}
		    }
		   	.fa-check-circle{
		   		display:inline-block;
			    vertical-align:top;
			    font-size:24px;
			    margin-right:4px;
		   	}
		   	.text{
		   		font-size: 12px;
		   	}
		}
	}
</style>