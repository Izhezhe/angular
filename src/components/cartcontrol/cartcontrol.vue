<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart">
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script>
  import Vue from 'vue'
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
          // 操作原来不存在的字段时，用Vue的set方法添加
        } else {
          this.food.count++
        }
        this.$emit('cartAdd', event.target)
        // 将当前对象发送到父组件
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return
        }
        if (this.food.count) {
          this.food.count--
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    .cart-decrease, .cart-add
      display inline-block
      padding 6px
      font-size 24px
      line-height 24px
      color rgb(0, 160, 220)
    .cart-decrease
      transition all 0.3s linear
      &.move-transition
        opacity 1
        transform translateX(0) rotate(0)
      &.move-enter, &.move-leave-active
        opacity 0
        transform translateX(24px) rotate(180deg)
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      text-align center
      padding-top 6px
      font-size 10px
      line-height 24px
      color rgb(147, 153, 159)
</style>