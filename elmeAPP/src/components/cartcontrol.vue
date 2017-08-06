<template>
  <div class="cartcontrol">
  	<div class="cart-decrease icon-remove_circle_outline" v-show='food.count>0' @click='decreaseCart'></div>
  	<div class="cart-count " v-show='food.count > 0'>{{food.count}}</div>
  	<div class="cart-add icon-add_circle" @click='addCart'></div>
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
  created () {
  	// console.log(this.food)
  },
  methods: {
  	addCart (event) {
  		if (!event._constructed) {
  			return 
  		}
  		console.log(this.food.count)
  		if (!this.food.count) {
  			Vue.set(this.food,'count',1)  // 使用vue.set接口添加obj key 更新视图
  		}else{
  			this.food.count ++
  		}
  	},

  	decreaseCart (event) {
  		if (!event._constructed) {
  			return 
  		}
  		if (this.food.count > 0) {
  			this.food.count --
  		}
  	}
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='less' rel="stylesheet/less" scpoed>
	.cartcontrol {
		font-size: 0;
		.cart-decrease, .cart-add{
			display: inline-block;
			line-height: 24px;
			font-size: 24px;	
			padding: 6px;
			color: rgb(0, 160, 220);
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
		}
	}

</style>
