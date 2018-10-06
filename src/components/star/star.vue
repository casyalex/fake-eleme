<template>
  <div class="star" :class="starType">
    <span v-for="(item, index) in itemClasses" :key="index" class="star-item" :class="itemClass"></span>
  </div>
</template>

<script>
export default {
  props: {
    size: {
      type: Number
    },
    score: {
      type: Number
    }
  },
  computed: {
    starType() {
      return 'star-' + this.size
    }
  }
}
</script>

<style lang="stylus" scoped>
@import "~@/common/stylus/mixin.styl";
.star
  font-size 0
  .star-item
    display inline-block
    background-repeat no-repeat
  &.star-48
    .star-item
      width 20px
      height 20px
      margin-right 22px
      background-size 20px 20px
      &.on
        bg-image('star48_on')
      &.half
        bg-image('star48_half')
      &.off
        bg-image('star48_off')

  &.star-36
  &.star-24

</style>


