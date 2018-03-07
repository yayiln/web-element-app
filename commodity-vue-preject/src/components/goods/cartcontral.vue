<template>
    <div class="cartcontral">
      <div class="decrease-item"  @click="decreaseCart"
           v-show="food.count">-
      </div>
      <div class="choose-number-item" v-show="food.count">{{food.count}}</div>
      <div class="add-item" @click="addCart">+</div>
    </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  export default {
    props:{
      food:{
        type:Object
      }
    },
    methods:{
      /**
       * 加
       */
      addCart(){
        if(!event._constructed){//阻止非vue事件
          return;
        }
        if(!this.food.count){
          Vue.set(this.food,'count',1);
        } else {
          this.food.count++;
        }
      },
      /**
       * 减
       */
      decreaseCart(){
        if(!event._constructed){//阻止非vue事件
          return;
        }
        if(this.food.count){
          this.food.count--;
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontral
    font-size 0
    .decrease-item
      display inline-block
      width 24px
      height 24px
      line-height 20px
      text-align center
      font-size 24px
      border-radius 50%
      background rgb(0,160,220)
      color white
    .choose-number-item
      display inline-block
      margin 0 5px 0 5px
      color rgb(147,153,159)
      padding 6px
      font-size 14px
    .add-item
      display inline-block
      width 24px
      height 24px
      line-height 20px
      text-align center
      font-size 24px
      border-radius 50%
      background rgb(0,160,220)
      color white

</style>
