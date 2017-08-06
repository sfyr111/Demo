<template>
  <div class="goods">
  	<div class="menu-wrapper" ref='menuWrapper'>
  		<ul>
  			<li v-for='(item,index) in goods' class='menu-item' :class="{current: currentIndex === index}" @click='selectMenu(index, $event)'>
  				<span class='text border-1px'>
  					<span v-show='item.type>0' class='icon' :class='classMap[item.type]'></span>{{item.name}}
  				</span>
  			</li>
  		</ul>
  	</div>
  	<div class="foods-wrapper" ref='foodsWrapper'>
  		<ul>
  			<li v-for='item in goods' class='food-list food-list-hook'>
  				<h1 class="title">{{item.name}}</h1>
  				<ul>
  					<li v-for='food in item.foods' class='food-item border-1px'>
  						<div class="icon">
  							<img width='57' height='57' :src="food.icon">
  						</div>
  						<div class="content">
  							<h2 class="name">{{food.name}}</h2>
  							<p class="desc">{{food.description}}</p>
  							<div class="extra">
  								<span class='count'>月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
  							</div>
  							<div class="price">
  								<span class='now'>￥{{food.price}}</span><span class='old' v-show='food.oldPrice'>￥{{food.oldPrice}}</span>
  							</div>
  							<div class="cartcontrol-wrapper">
  								<cartcontrol :food='food'></cartcontrol>
  							</div>
  						</div>
  					</li>
  				</ul>
  			</li>
  		</ul>
  	</div>
  	<shopcart :delivery-price='seller.deliveryPrice' :min-price='seller.minPrice'></shopcart>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopcart from './shopcart'
import cartcontrol from './cartcontrol'

const ERR_OK = 0

export default {
  name: 'goods',
  props: {
  	seller: {
  		type: Object
  	}
  },
  components: {
		shopcart,
		cartcontrol
  },
  data () {
    return {
      goods: [],
      classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      listHeight: [],
      scrollY: 0   // Y distance
    }
  },
  created () {
  	axios.get('api/goods').then((response) => {
  		let { data } = response
  		console.log(data)
  		if (data.errno === ERR_OK) {
  			this.goods = data.data
  			this.$nextTick(() => {
	  			this._initScroll()
	  			this._calculateHeight()
  			})
  		}
   	})

  },
  methods: {
  	_initScroll () {
  		this.meunScroll = new BScroll(this.$refs.menuWrapper, {
  			click: true   // 默认开启点击事件
  		})
  		this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
  			click: true,   // 默认开启点击事件
  			probeType: 3  // 监测实时滚动的位置
  		})
  		this.foodsScroll.on('scroll', (pos) => {
  			this.scrollY = Math.abs(Math.round(pos.y))
  		})
  	},
  	_calculateHeight () {
  		let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
  		let height = 0
  		this.listHeight.push(height)
  		for (let i = 0; i<foodList.length; i++) {
  			let item = foodList[i]
  			height += item.clientHeight
  			this.listHeight.push(height)
  		}
  	},
  	selectMenu (index, evnet) {
  		// event 为传入的原生dom事件
			if (!event._constructed) {
  			return 
  		}
  		let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
  		let el = foodList[index]
  		this.foodsScroll.scrollToElement(el, 300)
  	}
  },
  computed: {
  	currentIndex () {
  		for (let i = 0; i < this.listHeight.length; i++) {
  			let height1 = this.listHeight[i]
  			let height2 = this.listHeight[i + 1]
  			if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
  				return i
  			}
  		}
  		return 0
  	}
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='less' rel="stylesheet/less" scpoed>

@import '../common/stylus/mixin.less';

.goods {
	display: flex;
	position: absolute;
	top: 174px;
	bottom: 46px;
	width: 100%;
	overflow: hidden;
	.menu-wrapper {
		flex: 0 0 80px;
		width: 80px;
		background: #f3f5f7;
		.menu-item {
			display: table;
			height: 54px;
			width: 56px;
			line-height: 14px;
			padding: 0 12px;
			&.current {
				position: relative;
				z-index: 10;
				margin-top: -1px;
				background: #fff;
				font-weight: 700;
				.text {
					.border-none()
				}
			}
			.icon {
				display: inline-block;
				vertical-align: top;
				width: 12px;
				height: 12px;
				margin-right: 2px;
				background-size: 12px 12px;
				background-repeat: no-repeat;
				&.decrease {
					background-image: url('./goods/decrease_3@2x.png')
				}
				&.discount {
					background-image: url('./goods/discount_3@3x.png')
				}
				&.special {
					background-image: url('./goods/special_3@3x.png')
				}
				&.invoice {
					background-image: url('./goods/invoice_3@3x.png')
				}
				&.guarantee {
					background-image: url('./goods/guarantee_3@3x.png')
				}
			}
			.text {
				display: table-cell;
				vertical-align: middle;
				font-size: 12px;
				.border-1px(rgba(7, 17, 27, 0.1))
			}
		}
	}	
	.foods-wrapper {
		flex: 1;
		.title {
			padding-left: 14px;
			height: 26px;
			line-height: 26px;
			border-left: 2px solid #d9dde1;
			font-size: 12px;
			color: rgb(147, 153, 159);
			background: #f3f5f7;
		}
		.food-item {
			display: flex;
			margin: 18px;
			padding-bottom: 18px;
			.border-1px(rgba(7, 17, 27, 0.1));
		}
		&:last-child{
			.border-none();
			margin-bottom: 0;
		}
		.icon {
			flex: 0 0 57px;
			margin-right: 10px;
		}
		.content {
			flex: 1;
			.name {
				margin: 2px 0 8px 0
				height 14px;
				line-height: 14px;
				font-size: 14px;
				color: rgb(7, 17, 27);
			}
			.desc, .extra {
				line-height: 10px;
				font-size: 10px;
				color: rgb(147, 153, 159);
			}
			.desc {
				line-height: 12px;
				margin-bottom: 8px;
			}
			.extra {
				.count {
					margin-right: 12px;
				}
			}
			.price {
				font-weight: 700;
				line-height: 24px;
				.now {
					margin-right: 8px;
					font-size: 14px;
					color: rgb(240, 20, 20);
				}
				.old {
					text-decoration: line-through;
					font-size: 10px;
					color: rbb(147, 153, 159);
				}
			}
			.cartcontrol-wrapper {
				position: absolute;
				right: 0;
				bottom: 12px;
			}
		}
	}
}

</style>
