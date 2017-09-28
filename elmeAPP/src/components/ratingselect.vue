<template>
  <div class="ratingselect">
    <div class="rating-type border-1px">
      <span class='block  positive'>{{desc.all}}
        <span class='count'>
          </47span>
        </span>
        <span class='block  positive'>{{desc.positive}}
          <span class='count'>40</span>
        </span>
        <span class='block  negative'>{{desc.negative}}
          <span class='count'>7</span>
        </span>
    </div>
    <div class="switch">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  props: {
    ratings: {
      type: Array,
      default() {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },

    desc: {
      type: Object,
      default() {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  }
}

</script>

<style lang='less' rel="stylesheet/less" scpoed>
@import '../common/stylus/mixin.less';

.ratingselect {
  .rating-type {
    padding: 18px 0;
    margin: 0 18px;
    .border-1px(rgba(7, 17, 27, 0.1));
    font-size: 0;
  }
  .block {
    display: inline-block;
    padding: 8px 12px;
    margin-right: 8px;
    border-radius: 1px;
    color: rgb(77, 85, 93); // color: #fff;
    &.positive {
      color: rgba(77, 85, 93);
      background: rgba(0, 160, 0, 2);
    }
    &.negative {
      background: rgba(0, 160, 0, 2);
    }
  }
}
</style>


