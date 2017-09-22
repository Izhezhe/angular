<template>
  <transition name="move">
    <div class="food" v-show="showFlag" ref="foodWrapper">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="showFlag = !showFlag">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span><span v-if="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-warpper">
            <cartcontrol :food="food" @cartAdd="addCart"></cartcontrol>
          </div>
          <transition name="buy">
            <div class="buy" v-show="!food.count || food.count == 0" @click.stop.prevent="addFirst">加如入购物车</div>
          </transition>
        </div>
        <split></split>
      </div>
    </div>
  </transition>
</template>

<script>
  import Vue from 'vue'
  import BScroll from 'better-scroll'
  import cartcontrol from '@/components/cartcontrol/cartcontrol'
  import split from '@/components/split/split'

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data () {
      return {
        showFlag: false
      }
    },
    methods: {
      show () {
        this.showFlag = true
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.foodWrapper, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      },
      addFirst (event) {
        if (!event._constructed) {
          return
        }
        Vue.set(this.food, 'count', 1)
        this.$emit('cartAdd', event.target)
      },
      addCart (el) {
        this.$nextTick(() => {
          this.$emit('cartAdd', event.target)
        })
      }
    },
    components: {
      cartcontrol,
      split
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food
    position fixed
    top 0px
    left 0px
    bottom 48px
    z-index 30
    width 100%
    background #fff
    transition all 0.5s
    &.move-transition
      transform translate3d(0, 0, 0)    
    &.move-enter, &.move-leave-active
      transform translate3d(100%, 0, 0)
    .image-header
      position relative
      width 100%
      height 0
      padding-top 100%
      img
        position absolute
        top 0px
        left 0px
        width 100%
        height 100%
      .back
        position absolute
        top 5px
        left 0px
        .icon-arrow_lift
          display block
          padding 10px
          font-size 20px
          color #fff
    .content
      padding 18px
      position relative
      .title
        line-height 14px
        margin-bottom 8px
        font-size 14px
        font-weight 700
        color rgb(7, 17, 27)
      .detail
        margin-bottom 18px
        line-height 10px
        height 10px
        font-size 0
        .sell-count, .rating
          font-size 10px
          color rgb(147, 153, 159)
        .sell-count
          margin-right 12px
      .price
        font-weight 700
        line-height 24px
        .now
          margin-right 8px
          font-size 14px
          color rgb(240, 20, 20)
        .old
          text-decoration line-through
          font-size 10px
          color rgb(147, 153, 159)
      .cartcontrol-warpper
        position absolute
        right 12px
        bottom 12px
      .buy
        position absolute
        right 18px
        bottom 18px
        z-index 10
        height 24px
        line-height 24px
        padding 0 12px
        box-sizing border-box
        font-size 10px
        border-radius 12px
        color #fff
        background rgb(0, 160, 220)
        transition all 0.2s
        &.buy-transition
          opacity 1
        &.buy-enter, &.buy-leave-active
          opacity 0
</style>