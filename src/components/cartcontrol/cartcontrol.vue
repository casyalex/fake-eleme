<template>
  <div class="cartcontorl">
    <transition name="move">
      <div class="cart-minus icon-remove_circle_outline" v-show="food.count > 0" @click="minusCart"></div>
    </transition>
    <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script>
export default {
  props: {
    food: {
      type: Object
    }
  },
  created () {
    // console.log(this.food)
  },
  methods: {
    addCart () {
      if (!this.food.count) {
        this.$set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      this.$emit('cartadd', event.target)
    },
    minusCart () {
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
  .cartcontorl
    font-size 0
    .cart-minus,.cart-add
      display inline-block
      padding 6px
      line-height 24px
      font-size 24px
      color rgb(0,160,220)
      &.move-enter-active,&.move-leave-active
        opacity 1
        transform rotate(0)
        transition all 0.4s linear
      &.move-enter,&.move-leave-to
        opacity 0
        transform translate3d(24px,0,0) rotate(180deg)
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 6px
      line-height 24px
      text-align center
      font-size 10px
      color rgb(147,153,159)
    .cart-add
      display inline-block
</style>
