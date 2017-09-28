<template>
	<div class="cartcontrol">
		<transition name='move'>
			<div class="cart-decrease " v-show='food.count>0' @click.stop='decreaseCart'>
				<span class="inner icon-remove_circle_outline"></span>
			</div>
		</transition>
		<div class="cart-count " v-show='food.count>0'>{{food.count}}</div>
		<div class="cart-add icon-add_circle" @click.stop='addCart'></div>
	</div>
</template>
<script>

import Vue from 'vue'
export default {
	name: 'cartcontrol',
	props: {
		food: {
			type: Object
		}
	},
	created() {
		// console.log(this.food)
	},
	methods: {
		addCart(event) {
			if (!event._constructed) {
				return
			}
			// console.log(this.food.count)
			if (!this.food.count) {
				Vue.set(this.food, 'count', 1)  // 使用vue.set接口添加obj key 更新视图
			} else {
				this.food.count++
			}

			// emit to 父组件 goods
			this.$emit('addCart', event.target)
		},

		decreaseCart(event) {
			if (!event._constructed) {
				return
			}
			if (this.food.count > 0) {
				this.food.count--
			}
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='less' rel="stylesheet/less" scpoed>
.cartcontrol {
	font-size: 0;
	.cart-decrease {
		display: inline-block;
		padding: 6px;
		transition: all 0.4s linear;
		// 過渡效果
		&.move-enter-active, &.move-leave-active {
			opacity: 1;
			transform: translate3d(0, 0, 0);
		}
		&.move-enter, &.move-leave-to {
			opacity: 0;
			transform: translate3d(24px, 0, 0);
			.inner {
				transform: rotate(180deg);
			}
		}
		.inner {
			display: inline-block;
			line-height: 24px;
			font-size: 24px;
			color: rgb(0, 160, 220);
			transition: all 0.4s linear;
			transform: rotate(0);
		}
	}
	.cart-count {
		display: inline-block;
		vertical-align: top;
		width: 12px;
		padding-top: 6px;
		line-height: 24px;
		text-align: center;
		font-size: 10px;
		color: rgb(147, 153, 159);
	}
	.cart-add {
		display: inline-block;
		line-height: 24px;
		font-size: 24px;
		padding: 6px;
		color: rgb(0, 160, 220);
	}
}
</style>
