<template>
  <div class="header">
  	<div class="content-wrapper">
  		<div class="avatar">
  			<img width="64" height="64" :src="seller.avatar">
  		</div>

  		<div class="content">
				<div class="title">
	  			<span class="brand"></span>
	  			<span class="name">{{seller.name}}</span>
	  		</div>

	  		<div class="description">
	  			{{seller.description}} / {{seller.deliveryTime}}分钟送达
	  		</div>
	  		<div class="support" v-if='seller.supports'>
	  			<span class="icon" :class="classMap[seller.supports[0].type]"></span>
	  			<span class="text">{{seller.supports[0].description}}</span>
	  		</div>
  		</div>

      <div v-if='seller.supports' class="support-count">
      	<span class='count'>{{seller.supports.length}}个</span>
      	<i class='icon-keyboard_arrow_right'></i>
      </div>
  	</div>

  	<div class="bulletin-wrapper">
  		<span class="bulletin-title"></span>
  		<span class="bulletin-text">{{seller.bulletin}}</span>
    	<i class='icon-keyboard_arrow_right'></i>
  	</div>
  	<div class="background">
  		<img :src="seller.avatar" width=100% height=100%>
  	</div>	

  </div>
</template>

<script>
export default {
  props: {
  	seller: {
  		type: Object
  	}
  },
  data () {
    return {
			classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  },
  created () {

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='less' rel="stylesheet/less" scpoed>

@import '../common/stylus/mixin.less';

.header {
	position: relative;
	color: #fff;
	background: rgba(7, 17, 27, 0.5);
	.content-wrapper {
		padding: 24px 12px 18px 24px;
		position: relative;
		font-size: 0;
		.avatar {
			display: inline-block;
			vertical-align: top;
			img{
				border-radius: 2px;
			}
		}
		.content {
			display: inline-block;
			margin-left: 16px;
			.title{
				margin: 2px 0 8px 0;
				font-size: 0;
				.brand{
					display: inline-block;
					vertical-align: top;
					width: 30px;
					height: 18px;
					/*background-image: url('./header/brand@2x.png');*/
					.bg-image(); 
					background-size: 30px 18px;
					background-repeat: no-repeat;
 				}
 				.name{
 					font-size: 16px;
 					font-weight: bold;
 					line-height: 18px;
					margin-left: 6px;
 				}
			}
			.description {
				margin-bottom: 10px;
				line-height: 12px;
				font-size: 12px;
			}
			.support {
				.icon {
					display: inline-block;
					vertical-align: top;
					width: 12px;
					height: 12px;
					margin-right: 4px;
					background-size: 12px 12px;
					background-repeat: no-repeat;
					&.decrease {
						background-image: url('./header/decrease_1@2x.png')
					}
					&.discount {
						background-image: url('./header/discount_1@3x.png')
					}
					&.special {
						background-image: url('./header/special_1@3x.png')
					}
					&.invoice {
						background-image: url('./header/invoice_1@3x.png')
					}
					&.guarantee {
						background-image: url('./header/guarantee_1@3x.png')
					}
				}
				.text{
					line-height: 12px;
					font-size: 10px;
				}
			}
		}
		.support-count {
			position: absolute;
			right: 12px;
			bottom: 14px;
			padding: 0 8px;
			height: 24px;
			line-height: 24px;
			border-radius: 14px;
			background: rgba(0, 0, 0, 0.2);
			text-align: center;
			.count {
				vertical-align: top;
				font-size: 10px;
			}
			.icon-keyboard_arrow_right {
				margin-left: 2px;
				line-height: 24px;
				font-size: 10px;
			}
		}
	}
	.bulletin-wrapper {
		position: relative;
		height: 28px;
		line-height: 28px;
		padding: 0 22px 0 12px;
		white-space: nowrap;
		overflow: hidden;
		text-overflow:ellipsis;
		background: rgba(7, 17, 27, 0.1);
		.bulletin-title {
			display: inline-block;
			vertical-align: top;
			margin-top: 7px;
			width: 22px;
			height: 12px;
			background-image: url('./header/bulletin@3x.png');
			background-size: 22px 12px;
		}
		.bulletin-text {
			margin:0 4px;
			vertical-align: top;
			font-size: 10px;
		}
		.icon-keyboard_arrow_right {
			position: absolute;
			font-size: 10px;
			right: 12px;
			top: 50%;
			transform: translateY(-50%);
		}
	}
	.background {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		filter: blur(10px);
	}
}

</style>
