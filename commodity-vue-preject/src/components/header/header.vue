<template>
    <div class="header">
      <div class="header-content" >
        <div class="head-img">
          <img :src="seller.avatar" width="64" height="64"/>
        </div>
        <div class="content" @click="isShowDetail">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}
          </div>
          <div class="supports" v-if="seller.supports">
            <span class="icon-span"></span>
            <span class="text-span">{{seller.supports[0].description}}</span>
          </div>
        </div>
      </div>
      <div class="header-bulletin"  @click="isShowDetail">
        <span class="bulletin-title"></span>
        <span class="bulletin-tv">{{seller.bulletin}}</span>
      </div>
      <div class="head-background">
        <img :src="seller.avatar" width="100%" height="100%"/>
      </div>
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper clearfix">
          <div class="detail-wrapper-content">
            <h1 class="name">{{seller.name}}</h1>
            <star-view :size="48" :score="seller.score" class="wrapper-content-stare"></star-view>
            <div class="wrapper-content-title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[item.type]"/>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>

            <div class="wrapper-content-title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>

            <div class="wrapper-content-bulletin">
              <p class="content-bulletin">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-foot" @click="hideDetail">
          <img src="../../drawable/icon_close.png"/>
        </div>
      </div>
    </div>
</template>

<script type="text/ecmascript-6">
  import starView from '@/components/star/star'
  export default {
    data(){
      return{
        detailShow:false
      }
    },
    props:{
      seller:{}
    },

    methods:{
      isShowDetail(){
        this.detailShow=true
      },
      hideDetail(){
        this.detailShow=false
      }
    },
    components: { starView },
    created(){
      this.classMap = ['decrease','discount','special','invoice','guarantee']
    }

  }
  function getSeller () {
    console.log(seller)
  }

</script>
<style lang="stylus" rel="stylesheet/stylus">
   @import "../../common/stylus/mixin"
   @import "../../common/stylus/base"
  .header{
    color: #fff;
    overflow hidden
    background :rgba(7,17,27,0.5);
    position relative
  }
  .header-content{
    padding: 24px 12px 18px 24px;
    font-size: 0;
  }
  .head-img{
    display: inline-block;
    vertical-align top;
  }
   .head-img img{
     border-radius 2px;
   }
  .content{
    font-size: 14px;
    display: inline-block;
    margin-left: 16px;
  }
  .title{
    margin: 2px 0 8px 0;
  }
  .brand{
    display: inline-block;
    vertical-align bottom
    width: 30px;
    height: 18px;
    bg-image('../../drawable/brand')
    background-size 30px 18px;
  }
  .name{
    margin-left 6px;
    font-size 16px;
    line-height 18px;
    font-weight bold
  }
  .description{
    margin-bottom 10px;
    line-height 12px;
    font-size 12px;

  }
    .icon-span{
      display inline-block;
      vertical-align top
      width 12px;
      height 12px;
      margin-right 2px;
      background-size 12px 12px;
      background-repeat no-repeat;
      bg-image('../../drawable/decrease_1')
 /*     &.decrease{
        bg-image('../../drawable/decrease_1')
      }
      &.discount{
        bg-image('../../drawable/discount_1')
      }
      &.guarantee{
        bg-image('../../drawable/guarantee_1')
      }*/
    }
  .text-span{
    vertical-align top
    line-height 12px;
    font-size 12px;
  }
  .header-bulletin
    height 28px
    line-height 28px;
    padding 0 22px 0 22px
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    .bulletin-title
      margin-top 8px
      display inline-block
      width 22px
      height 12px
      bg-image('../../drawable/bulletin')
      background-size 22px 12px
    .bulletin-tv
      vertical-align top
      margin-bottom 5px
      font-size 12px
      margin 0 4px
  .head-background{
   position absolute
   top 0;
   left 0;
   width 100%;
   height 100%;
   z-index -1;
   filter blur(10px)//模糊效果
 }

  .detail
    position fixed
    top 0
    left 0
    width 100%
    height 100%
    background rgba(7,17,27,0.8)
    .detail-wrapper
      min-height 100%
      width 100%
      .detail-wrapper-content
        margin-top 64px
        padding-bottom 64px
        .name
          line-height 16px;
          text-align center
          font-size 16px
        .wrapper-content-stare
          margin 18px
          text-align center
          padding 2px 0
        .wrapper-content-title
          display flex
          width 80%
          margin 10px auto 24px auto
          .line
              flex 1
              position relative
              top -6px
              border-bottom 1px solid rgba(255,255,255,0.2)
          .text
              padding 0 12px
              font-size 14px
        .wrapper-content-bulletin
          width 80%
          margin 24px auto 24px auto
          .content-bulletin
            font-size 12px
            font-weight 200px
            color rgb(255,255,255)
            padding 0 12px
            line-height 24px
            margin-right 12px
            margin-left 12px
        .supports
          width 80%
          margin 0 auto 20px auto
          .support-item
            padding 0 12px;
            margin-bottom 12px
            font-size 0
            &:last-child
               margin-bottom 0
            .icon
              display inline-block
              width 16px
              height 16px
              vertical-align top
              margin-right 6px
              background-size 16px 16px
              &.decrease{
                bg-image('../../drawable/decrease_2')
              }
              &.discount{
                bg-image('../../drawable/discount_2')
              }
              &.special{
                bg-image('../../drawable/special_2')
              }
              &.invoice{
                bg-image('../../drawable/invoice_2')
              }
              &.guarantee{
                bg-image('../../drawable/guarantee_2')
              }
            .text
              line-height 12px
              font-size 12px

   .detail-foot{
     position relative
     width 22px
     height 22px
     margin -44px auto 0 auto
     clear both
     font-size 12px
   }
   .detail-foot img{
     width 22px
     height 22px
   }
  .name{
    line-height 16px;
    text-align center
    font-size 14px
  }
</style>
