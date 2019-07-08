<template>
	<div class="cartcontrol">
			<transition name="move">
				<div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
						<span class="inner icon-remove_circle_outline"></span>
				</div>
			</transition>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
	</div>
</template>

<script>
	import Vue from 'vue';
	// 导出
	export default{
		props:{
			// 从父级传入food
			food:{
				type:Object
			}
		},
		methods:{
			addCart(event){
				// // 解决PC点击问题，判断是不是自己开发的
				if(!event._constructed){
					return;
				}
				if(!this.food.count){
					// 通过接口设置,food.count=1不可行-->对象是新创的
					Vue.set(this.food, 'count', 1);
				}else{
					this.food.count ++;
				}
				this.$emit('cart-add', event.target);
			},
			decreaseCart(event){
				if(!event._constructed){
					return;
				}
				if(this.food.count){
					this.food.count --;
				}
			}
		}
	};
</script>

<style scoped lang="stylus">
	.cartcontrol
		font-size:0
		.cart-decrease
			display:inline-block
			padding:6px
			transform translate3d(0, 0, 0)
			opacity: 1
			.inner
				display inline-block
				font-size:24px
				line-height:24px
				color:rgb(0,160,220)
				transform rotate(0)
			&.move-enter-active, &.move-leave-active
				transition all 0.4s linear
			&.move-enter, &.move-leave-to
				opacity 0
				transform translate3d(24px, 0 ,0) rotate(-180deg)
		.cart-count
			display:inline-block
			vertical-align top
			width 12px
			padding-top 6px
			line-height 24px
			text-align center
			font-size 10px
			color rgb(147,153,159)
		.cart-add
			display:inline-block
			padding:6px
			font-size:24px
			line-height:24px
			color:rgb(0,160,220)
</style>
