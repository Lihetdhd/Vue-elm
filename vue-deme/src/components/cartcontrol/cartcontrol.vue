<template>
	<div class="cartcontrol">
		<transition name="fade">
			<div class="cart-decres" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)">
				<transition name="inner">
					<i class="inner fa fa-minus-circle"></i>
				</transition>
			</div>
		</transition>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add" @click.stop.prevent="addCart($event)">
			<i class="fa fa-plus-circle"></i>
		</div>
	</div>
</template>
<script type="text/javascript">
	import Vue from 'vue';
	export default{
		props:{
			food:{
				type:Object
			}
		},
		data(){
			return{
				
			}
		},
		methods:{
			addCart(event){
				if(!event._constructed){
					return;
				}
				if(!this.food.count){
					Vue.set(this.food,'count',1);
				}else{
					this.food.count++;
				}
				this.$emit('increment',event.target);	
			},
			decreaseCart(event){
				if(!event._constructed){
					return;
				}
				this.food.count--;
			}
		}
	}
</script>
<style scoped lang="less">
@import url('../../common/css/font-awesome.min.css');
	.cartcontrol{
		font-size:0;
		.cart-decres, .cart-add{			
			display: inline-block;
			padding:6px;
			color: rgb(147,153,159);
			line-height:24px;
			font-size:24px;
			&.fade-enter-active, &.fade-leave-active{
				transition: all 0.4s linear;
			}
			&.fade-enter, &.fade-leave-active{
				opacity: 0;
				transform:translate3d(24px,0,0);
			}
			.inner{
				display: inline-block;
				line-height: 24px;
				font-size: 24px;
				vertical-align: top;
				&.inner-enter-active, &.inner-leave-active{
					transition: all 0.4s linear;
					transform: rotate(0);
				}
				&.inner-enter, &.inner-leave-active{
					opacity: 0;
					transform: rotate(180deg);
				}
			}
		}
		.cart-count{
			display: inline-block;
			font-size: 10px;
			width: 12px;
			color: rgb(147,153,159);
			line-height:36px;
			vertical-align: top; 
			text-align: center;
		}
		.cart-add{
			color: rgb(0,160,220);
			display: inline-block;
			line-height:24px;
			font-size:24px;
		}	
	}
</style>,