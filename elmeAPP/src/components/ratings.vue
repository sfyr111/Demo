<template>
  <div class="ratings" ref='ratings'>
    <div class="ratings-contnet">
      <div class="overview">
        <div class="overview-left">
          <h1 class='score'>{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size='36' :score='seller.serviceScore' />
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size='36' :score='seller.foodScore' />
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class='delivery'>{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect :selectType='selectType' @select='select' @toggle='toggleContent' :onlyContent='onlyContent' :ratings='ratings'></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for='rating in ratings' v-show='needShow(rating.rateType,rating.text)' class='rating-item' :key='rating.id'>
            <div class="avatar">
              <img width=28 height=28 :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size='24' :score='rating.score'></star>
                <span class="delivery" v-show='rating.deliveryTime'>{{rating.deliveryTIme}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show='rating.recommend && rating.recommend.length'>
                <span class="icon-thumb_up"></span>
                <span class='item' v-for='item in rating.recommend' :key='item.id'>{{item}}</span>
                <div class="time">{{rating.rateTime | formatDate}}</div>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>

import BScroll from 'better-scroll'
import { formatDate } from '../common/js/date'

import star from './star/star'
import split from './split'
import ratingselect from './ratingselect'

const ALL = 2

export default {
  name: 'ratings',
  components: {
    star,
    split,
    ratingselect
  },
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: true,
      ratings: []
    }
  },

  created() {
    axios.get('api/ratings').then((res) => {
      let { data } = res
      if (data.errno === 0) {
        this.ratings = data.data
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.ratings, {
            click: true
          })
        })
      }
    })
  },
  filters: {
    formatDate(time) {
      let date = new Date(time)
      // 把通用的方法放在抽象出来放在 common 目录下
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  methods: {
    needShow(type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    },
    select(type) {
      this.selectType = type
      this.$nextTick(() => {
        // better-scroll 更新
        this.scroll.refresh()
      })
    },

    toggleContent() {
      console.log('toggle')
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        // better-scroll 更新
        this.scroll.refresh()
      })
    }
  }
}
</script>

<style lang='less' rel="stylesheet/less" scpoed>
@import '../common/stylus/mixin.less';


.ratings {
  position: absolute;
  top: 174px;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
  .overview {
    display: flex;
    padding: 18px 0;
    .overview-left {
      flex: 0 0 137px;
      width: 137px;
      padding: 6px 0;
      border-right: 1px solid rgba(7, 17, 27, 0.1);
      text-align: center;

      @media only screen and (max-width: 320px) {
        flex: 0 0 120px;
        width: 120px;
      }
      .score {
        margin-bottom: 6px;
        line-height: 28px;
        font-size: 24px;
        color: rgb(255, 153, 0);
      }
      .title {
        margin-bottom: 8px;
        line-height: 12px;
        font-size: 12px;
        color: rgb(7, 17, 27);
      }
      .rank {
        line-height: 10px;
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
    }
    .overview-right {
      flex: 1;
      padding: 6px 0 6px 24px;
      @media only screen and (max-width: 320px) {
        padding-left: 6px;
      }
      .score-wrapper {
        margin-bottom: 8px;
        font-size: 0;
        .title {
          line-height: 18px;
          display: inline-block;
          font-size: 12px;
          vertical-align: top;
          color: rgb(7, 17, 27);
        }
        .star {
          display: inline-block;
          vertical-align: top;
          margin: 0 12px;
        }
        .score {
          line-height: 18px;
          display: inline-block;
          vertical-align: top;
          font-size: 12px;
          color: rgb(255, 153, 0);
        }
        .delivery-wrapper {
          font-size: 0;
          .title {
            line-height: 18px;
            font-size: 12px;
            color: rgb(7, 17, 27);
          }
          .delivery {
            margin-left: 12px;
            font-size: 12px;
            color: rgb(147, 153, 159);
          }
        }
      }
    }
  }
  .rating-wrapper {
    padding: 0 18px;
    .rating-item {
      display: flex;
      padding: 18px 0;
      .border-1px(rgba(7, 17, 27, 0.1));
      .avatar {
        flex: 0 0 28px;
        width: 28px;
        margin-right: 12px;
        img {
          border-radius: 50%;
        }
      }
      .content {
        flex: 1;
        position: relative;
        .name {
          margin-bottom: 4px;
          line-height: 12px;
          font-size: 10px;
          color: rgb(7, 17, 27);
        }
        .star-wrapper {
          margin-bottom: 6px;
          font-size: 0;
          .star {
            display: inline-block;
            vertical-align: top;
            margin-right: 6px;
          }
          .delivery {
            display: inline-block;
            vertical-align: top;
            margin-right: 6px;
            line-height: 12px;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
        }
        .text {
          line-height: 18px;
          color: rgb(7, 17, 27);
          font-size: 12px;
          margin-bottom: 8px;
        }
        .recommend {
          line-height: 16px;
          font-size: 0;
          .icon-thumb_up,
          .item {
            display: inline-block;
            margin: 0 8px 4px 0;
            font-size: 9px;
          }
          .icon-thumb_up {
            color: rgb(0, 160, 220);
          }
          .item {
            padding: 0 6px;
            border: 1px solid rgba(7, 17, 27, 0.1);
            border-radius: 1px;
            color: rgb(147, 153, 159);
            background: #fff;
          }
          .time {
            position: absolute;
            top: 0;
            right: 0;
            line-height: 12px;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
        }
      }
    }
  }
}
</style>
