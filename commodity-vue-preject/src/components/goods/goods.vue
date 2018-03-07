<template>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <!--vue2.0不能用$index-->
          <li v-for="(item, index) in goods" class="menu-item" :class="{'current':currentIndex === index}"
              @click="selectMeun(index)">
            <span class="text">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
            </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li v-for="item in goods" class="food-list food-list-hook">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="food in item.foods" class="food-item">
                <div class="icon">
                  <img width="57px" height="57px" :src="food.icon"/>
                </div>
                <div class="content">
                  <p class="name">{{food.name}}</p>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">¥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontral-wrapper">
                    <cartcontral :food="food"></cartcontral>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :select-foods="selectFoods"></shopcart>
    </div>
</template>

<script type="text/ecmascript-6">
  import mScroll from 'better-scroll'
  import shopcart from '@/components/shopcart/shopcart'
  import cartcontral from '@/components/goods/cartcontral'
  export default {
    data(){
      return{
        goods:[],
        listHight:[],
        scrollY:0
      };
    },
    props:{
      seller:{
        type:Object
      }
    },
    computed:{
      //联动取值
      currentIndex(){
        for (let i = 0;i<this.listHight.length;i++){
          let height1 = this.listHight[i];
          let height2 = this.listHight[i+1];
          if(!height2||(this.scrollY>=height1&&this.scrollY<height2)){
            return i;
          }
        }
        return 0;
      },
      selectFoods(){
        let foods = [];
        this.goods.forEach(function(good){
          good.foods.forEach(function(food){
            if(food.count){
              foods.push(food)
            }
          });
        });
        return foods;
      }
    },
    created() {
      this.classMap = ['decrease','discount','special','invoice','guarantee'];

      this.$http.get('/api/goods').then(response => {
        var mData = response.body;
        this.goods = mData.data;
        this.$nextTick(()=>{
         this.initScroll();
         this.calculateHeight();
        });
      }, response => {
      });
    },
    components: {
      shopcart,
      cartcontral
    },

    methods:{
      /**
       * 点击左边 右边联动
       */
      selectMeun(index){
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el,300);
      },
      /**
       * 滚动
       */
      initScroll(){
        this.menuScroll = new mScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new mScroll(this.$refs.foodsWrapper, {
          probeType:3,
          click: true
        });
          this.foodsScroll.on('scroll',(pos)=>{
          this.scrollY = Math.abs(Math.round(pos.y))
        });
      },
      /**
       * 获取高度
       */
      calculateHeight(){
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let hight = 0;
        this.listHight.push(hight);
        for(let i=0;i<foodList.length;i++){
          let item = foodList[i];
          hight+=item.clientHeight;//获取每个区间的高度
          this.listHight.push(hight);
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin"
  .goods
    display flex
    z-index -1
    position absolute
    width 100%
    top 172px
    bottom 46px
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      width 80px
      background #f3f5f7
      .menu-item
        display table//垂直居中最好的布局
        height 54px
        width 56px
        line-height 14px
        padding 0 12px
        &.current
          position relative
          z-index 10px
          margin-top -1px
          background #ffffff
          font-size 700px
          .text
            border-bottom 0px solid rgba(7,17,27,0.1)
        .icon
          display inline-block
          width 12px
          height 12px
          margin-right 2px
          vertical-align top
          background-size 12px 12px
          &.decrease{
            bg-image('../../drawable/decrease_3')
          }
          &.discount{
            bg-image('../../drawable/discount_3')
          }
          &.special{
            bg-image('../../drawable/special_3')
          }
          &.invoice{
            bg-image('../../drawable/invoice_3')
          }
          &.guarantee{
            bg-image('../../drawable/guarantee_3')
          }
        .text
          display table-cell
          width 56px
          vertical-align middle
          font-size 12px
          border-bottom 1px solid rgba(7,17,27,0.1)


    .foods-wrapper
      flex 1
      background #fff
      .title
        display flex
        align-items: center;
        padding-left 14px
        height 26px
        border-left 2px solid#d9dde1
        font-size 12px
        color rgb(147,153,159)
        background #f3f5f7
      .food-item
        display flex
        position relative
        margin 18px
        padding-bottom 18px
        border-bottom 1px solid rgba(7,17,27,0.1)
        .content
          flex 1
        .name
          display inline-block
          margin 2px 0 8px 0
          height 14px
          line-height 14px
          font-size 14px
          color rgb(7,17,27)
        .desc,.extra
          line-height 10px
          font-size 10px
          color rgb(147,153,159)
          margin-bottom 8px
        .extra
          .count
            margin-right 12px
        .icon
          flex 0 0 57px
        .price
          font-weight 700px
          line-height 20px
          .now
            margin-right 8px
            font-size 14px
            color red
          .old
            text-decoration line-through//添加横线
            font-size 10px
            color rgb(147,153,159)

        .cartcontral-wrapper
          position absolute
          bottom  12px
          right 0
</style>
