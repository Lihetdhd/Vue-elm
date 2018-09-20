<template>
	<div class="star" >
		<div class="star-item" :class ="starType">
			<span v-for="item in itemClasses" class="star-item" :class="item" ></span>
		</div>
	</div>
</template>
<script type="text/javascript">
	const Length = 5;
	const Cls_on = 'on';
	const Cls_half = 'half';
	const Cls_off = 'off';

	export default{
		data(){
			return{

			}
		},
		props:{
			score:{
				type:Number
			},
			size:{
				type:Number
			}
		},
		computed:{
			starType(){
				return 'star-' + this.size;
			},
			itemClasses(){
				let result = [];
				let score = Math.floor(this.score*2)/2;
				let hasDecimal = score % 1 !==0;
				let integer = Math.floor(score);
				for(let i=0;i<integer;i++){
					result.push(Cls_on)
				}
				if(hasDecimal){
					result.push(Cls_half);
				}
				while(result.length<Length){
					result.push(Cls_off);
				}
				return result;
				
			}
		}	
	};
</script>
<style scoped lang="less">
	.star{
		font-size:0;
		text-align: center;
		margin:18px 0px 28px 15px;
		height: 16px;
		.star-48{
			.star-item{
				display: inline-block;
				background-repeat:no-repeat;
				width: 20px;
				height: 20px;
				margin-right: 22px;
				background-size: 20px 20px;
				&.on{
				background-image: url(../../../resource/img/star36_on@2x.png);
				}
				&.half{
					background-image: url(../../../resource/img/star36_half@2x.png);
				}
				&.off{
					background-image: url(../../../resource/img/star36_off@2x.png);
				}
			}
			
			
		}
		.star-36{
			.star-item{
				width:15px;
		        height:15px;
		        margin-right:3px;
		        display: inline-block;
		        background-size:15px 15px;
		        &.on{
					background-image: url(../../../resource/img/star36_on@2x.png);
				}
				&.half{
					background-image: url(../../../resource/img/star36_half@2x.png);
				}
				&.off{
					background-image: url(../../../resource/img/star36_off@2x.png);
				}
			}
			
		}
		.star-24{
			.star-item{
				width:10px;
		        height:10px;
		        margin-right:3px;
		        display: inline-block;
		        background-size:10px 10px;
		        &.on{
					background-image: url(../../../resource/img/star24_on@2x.png);
				}
				&.half{
					background-image: url(../../../resource/img/star24_half@2x.png);
				}
				&.off{
					background-image: url(../../../resource/img/star24_off@2x.png);
				}
			}
		}
	}
</style>