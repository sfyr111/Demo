<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <div class="content">
      <!-- 我是content -->
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
  import header from './components/header'

  const ERR_OK = 0

  export default {
    name: 'app',
    components: {
      'v-header': header
    },
    data () {
      return {
        seller: {}
      }
    },
    created () {
      axios.get('/api/seller').then((res) => {
        let response = res.data
        console.log(response)
        if (response.errno === ERR_OK) {
          this.seller = response.data
          console.log(this.seller)
        }
      })
    }
  }
</script>

<style lang='less' rel="stylesheet/less">

@import './common/stylus/mixin.less';

#app{
  .tab{
    display: flex;
    width: 100%;
    height: 40px; 
    line-height: 40px; 
    display: flex;
    .border-1px(rgba(7, 17, 27, 0.1));
    .tab-item{
      flex: 1;
      text-align: center;
      >a {
        display: block;
        font-size: 14px;
        color: rgb(77, 85, 93);
        &.active {
          color: rgb(240, 20, 20)
        }
      }
    }
  }
}



</style>
