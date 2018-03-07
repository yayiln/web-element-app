<template>
    <div class="shopcart">
      <div class="content">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo" :class="{'hight-light':totalcount>0}">
              <img src="../../drawable/shop_light.png" class="icon-shopping" :class="{'icon-light':totalcount>0}">
            </div>
            <div class="num" v-show="totalcount>0">{{totalcount}}</div>
          </div>
          <div class="price" :class="{'hight-light':totalcount>0}">¥{{totalPrice}}</div>
          <div class="desc" :class="{'hight-light':totalcount>0}">另需配送费¥{{deliveryPrice}}</div>
        </div>
        <div class="content-right">
          <span class="text" :class="payClass">{{payDesc}}</span>
        </div>
      </div>
    </div>
</template>

<script type="text/ecmascript-6">
  export default {
    data(){
      return{

      }
    },
    props:{
      selectFoods:{
        type:Array,
      },
      deliveryPrice:{
        type:Number,
      },
      minPrice:{
        type:Number,
      }
    },

    computed:{
      totalPrice(){
        let total = 0;
        if(!this.selectFoods){
          return total;
        }
        this.selectFoods.forEach((food)=> {
          total+=food.price*food.count;
        });
        return total;
      },
      totalcount(){
        let counts = 0;
        if(!this.selectFoods){
          return counts;
        }
        this.selectFoods.forEach((food)=> {
          counts+=food.count;
        });
        return counts;
      },
      payDesc(){
        if(this.totalPrice===0){
          return '¥'+this.minPrice+'起送';
        } else if(this.totalPrice<this.minPrice){
          return '还差¥'+(this.minPrice-this.totalPrice)+'起送';
        } else {
          return "去结算"
        }
      },
      payClass(){
        if(this.totalPrice<this.minPrice){
          return "not-enough";
        } else {
          return "enough"
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
.shopcart
  position fixed
  left 0
  bottom 0
  z-index 50
  width 100%
  height 48px
  background #141d27
  .content
    display flex
    background #141d27
    .content-left
      flex 1
      .logo-wrapper
        background #141d27
        display inline-block
        position relative
        top -10px
        margin 0 12px
        padding 6px
        width 56px
        height 56px
        box-sizing border-box
        vertical-align top
        border-radius 50%
        .logo
          width 100%
          height 100%
          border-radius 50%
          text-align center
          background #2b343c
          &.hight-light
            background rgb(0,160,220)
          .icon-shopping
            margin-top 8px
            height 25px
            width 25px
            /*background-image :url("../../drawable/shop.png")*/
            /*&.icon-light*/
              /*height 25px*/
              /*width 25px*/
              /*background-image :url("../../drawable/shop.png")*/
        .num
          position absolute
          top 0
          right 0
          width 24px
          height 16px
          line-height 16px
          text-align center
          border-radius 16px
          font-size 13px
          font-weight 700
          background red
          color white
          box-shadow :0 4px 8px 0 rgba(0,0,0,0.4)
      .price
        display inline-block
        vertical-align top
        line-height 24px
        margin-top 12px
        padding-right 12px
        box-sizing border-box
        border-right 1px solid rgba(255,255,255,0.1)
        font-size 16px
        font-weight 700
        color rgba(255,255,255,0.4)
        &.hight-light
          color white
      .desc
        display inline-block
        vertical-align top
        line-height 24px
        margin-top 12px
        margin-left 12px
        font-size 12px
        font-weight 300
        color rgba(255,255,255,0.4)
        &.hight-light
          color white
    .content-right
      position relative
      flex 0 0 105px
      width 105px
      background #2b343c
      .text
        position absolute
        top 0
        left 0
        text-align center
        width 100%
        height 100%
        line-height 48px
        font-size 14px
        font-weight 400
        color rgba(255,255,255,0.4)
        &.not-enough
          background #2b333b
        &.enough
          background #00b43c
          color white



</style>
