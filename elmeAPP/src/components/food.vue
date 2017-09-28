<template>
  <transition name="move">
    <div class="food" v-show='showFlag' ref='elFood'>
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image" alt="">
          <div class="back" @click='hide'>
            <i class='icon-arrow_lift' />
          </div>
        </div>
        <div class="content">
          <div class="title">{{food.name}}</div>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class='now'>￥{{food.price}}</span>
            <span class='old' v-show='food.oldPrice'>￥{{food.oldPrice}}</span>
          </div>

          <div class="cartcontrol-wrapper">
            <cartcontrol :food='food' @addCart="addCart"></cartcontrol>
          </div>
          <div class="buy" v-show='!food.count || food.count===0' @click='addFirst'>
            加入购物车
          </div>
        </div>

        <split v-show='food.info' />

        <div class="info" v-show='food.info'>
          <h1 class="title">商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>

        <split/>

        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect :selectType='selectType' :onlyContent='onlyContent' :desc='desc' :ratings='food.ratings' />
        </div>

      </div>
    </div>
  </transition>
</template>

<script>


import BScroll from 'better-scroll'
import cartcontrol from './cartcontrol'
import split from './split'
import ratingselect from './ratingselect'
import Vue from 'vue'

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  components: {
    cartcontrol,
    ratingselect,
    split
  },
  props: {
    food: {
      type: Object
    }
  },
  data() {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽',
      }
    }
  },
  methods: {
    show() {
      this.showFlag = true
      this.selectType = ALL
      this.onlyContent = true

      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.elFood, {
            click: true
          });
        } else {
          this.scroll.refresh()
        }
      })
    },
    hide() {
      this.showFlag = false
    },

    addCart(target) {
      this.$emit('addCart', event.target)
    },

    addFirst(event) {
      if (!event._constructed) {
        return
      }
      // this.$dispatch('cart.add',event.target)
      this.$emit('addCart', event.target)
      Vue.set(this.food, 'count', 1)
    }
  }
}
</script>

<style lang='less' rel="stylesheet/less" scpoed>
.food {
  position: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  width: 100%;
  z-index: 30;
  background: #fff;
  &.move-enter-active,
  &.move-leave-active {
    transition: .5s;
    transform: translate3D(0, 0, 0);
  }
  &.move-enter,
  &.move-leave-to {
    transform: translate3D(100%, 0, 0)
  }
  .image-header {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
    img {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }
    .back {
      position: absolute;
      top: 10px;
      left: 0;
      .icon-arrow_lift {
        display: block;
        padding: 10px;
        font-size: 20px;
        color: #fff;
      }
    }
  }
  .content {
    position: relative;
    padding: 18px;
    .title {
      line-height: 14px;
      margin-bottom: 8px;
      font-size: 14px;
      font-weight: 700;
      color: rgb(7, 17, 27);
    }
    .detail {
      margin-bottom: 18px;
      line-height: 10px;
      font-size: 0;
      height: 10px;
      .sell-count,
      .rating {
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
      .sell-count {
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
      bottom: 12px;
      right: 12px;
    }
    .buy {
      position: absolute;
      right: 18px;
      bottom: 18px;
      z-index: 10;
      height: 24px;
      line-height: 24px;
      padding: 0 12px;
      box-sizing: border-box;
      font-size: 10px;
      border-radius: 12px;
      color: #fff;
      background-color: rgb(0, 160, 220)
    }
    &.fade-enter-active,
    &.fade-leave-active {
      transition: 1s;
    }
    &.fade-enter,
    &.fade-leave-to {
      opacity: 0;
    }
  }
  .info {
    padding: 18px;
    .title {
      line-height: 14px;
      margin-bottom: 6px;
      font-size: 14px;
      color: rgb(7, 17, 27);
    }
    .text {
      line-height: 24px;
      padding: 0 8px;
      font-size: 12px;
      color: rgb(77, 85, 93);
    }
  }
  .rating {
    padding-top: 18px;
    .title {
      line-height: 14px;
      margin-bottom: 6px;
      margin-left: 18px;
      font-size: 14px;
      color: rgb(7, 17, 27);
    }
  }
}
</style>


