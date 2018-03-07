<template>
    <div class="seller">
      <div class="seller-content">
        <div class="over-view">
          <h1 class="title">{{seller.name}}</h1>
          <div class="desc">
            <star-view :size="36" :score="seller.score" class="star"></star-view>
          </div>
        </div>
      </div>
    </div>
</template>

<script type="text/ecmascript-6">
  import starView from '@/components/star/star'
  export default {
    props:{
      seller:{}
    },
    components: { starView }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">

</style>
