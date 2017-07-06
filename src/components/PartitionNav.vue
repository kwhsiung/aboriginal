<template>
  <table class="partition-nav">
    <tr>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-top-left', { 'active': index === '1' }]">1</div>
      </td>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-top-left', { 'active': index === '2' }]">2</div>
      </td>
    </tr>
    <tr>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-top-left', { 'active': index === '3' }]">3</div>
      </td>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-top-left', { 'active': index === '4' }]">4</div>
      </td>
    </tr>
  </table>
</template>

<script>
  export default {
    props: ['index']
  }
</script>

<style lang="stylus">
.partition-nav
  width 70px
  border-collapse collapse
  border 1px solid white
  
  &__row
    width 30%
    position relative
    &:after
      content ''
      display block
      margin-top 100%

  &__content
    position absolute
    top 0
    bottom 0
    left 0
    right 0
    display flex
    justify-content center
    align-items center
    color white
    &.active
      background white
      color black
    &-top-left
      border-right 1px solid white
      border-bottom 1px solid white
    &-top-right
      border-left 1px solid white
      border-bottom 1px solid white
    &-bottom-left
      border-right 1px solid white
      border-top 1px solid white
    &-bottom-right
      border-left 1px solid white
      border-top 1px solid white
</style>
