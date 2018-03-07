<template>
  <div>
    <header-view :seller="seller"></header-view>
    <div class="app-head-tab">
      <div class="tab-item">
        <router-link to="goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="seller">商家</router-link>
      </div>
    </div>
    <div class="app-content"><router-view :seller="seller"></router-view></div>
  </div>
</template>
<script type="text/ecmascript-6">
  import headerView from '@/components/header/header'
  const ERR_OK = 0;
  export default {
    data() {
      return{
        seller:{}
      }
    },
    created() {
      this.$http.get('/api/seller').then(response => {
        var someData = response.body;
        this.seller = someData.data;
      }, response => {
        console.log("请求出错")
      });
    },
    components: { headerView }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .app-head-tab{
    width: 100%;
    height: 40px;
    line-height: 40px;
    display :flex;
    border-bottom: 1px solid #eeeeee
  }
  .tab-item{
    flex :1;
    text-align: center;
    font-size :14px;
    color: rgb(77,85,93)
  }
  .router-link-active{
    color: rgb(240,20,20)
  }
</style>

