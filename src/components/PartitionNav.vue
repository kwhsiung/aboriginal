<template>
  <table class="partition-nav partition-nav--dark">
    <tr>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-top-left partition-nav__content-top-left--dark', { 'partition-nav__content--active--dark': index === 1 }, { 'partition-nav__content--active': index === 1 }]">1</div>
      </td>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-top-right partition-nav__content-top-right--dark', { 'partition-nav__content--active--dark': index === 2 }, { 'partition-nav__content--active': index === 2 }]">2</div>
      </td>
    </tr>
    <tr>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-bottom-left partition-nav__content-bottom-left--dark', { 'partition-nav__content--active--dark': index === 3 }, { 'partition-nav__content--active': index === 3 }]">3</div>
      </td>
      <td class="partition-nav__row">
        <div :class="['partition-nav__content partition-nav__content-bottom-right partition-nav__content-bottom-right--dark', { 'partition-nav__content--active--dark': index === 4 }, { 'partition-nav__content--active': index === 4 }]">4</div>
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
  width 40px
  border-collapse collapse
  cursor pointer
  &--dark
    transition border-color .5s ease-out
    border 1px solid white
  &--light
    transition border-color .5s ease-out
    border 1px solid #142d64
  
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
    font-size 12px
    // background-color transparent
    &--dark
      color white
    &--light
      color #142d64
    &--active
      transition background-color .5s ease-out, color .5s ease-out
      // background-color white
      // color black
      &--dark
        // transition background-color .5s ease-out, color .5s ease-out
        background-color white
        color black
      &--light
        // transition background-color .5s ease-out, color .5s ease-out
        background-color #142d64
        color white
    &-top-left
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-right thin solid white
        border-bottom thin solid white
      &--light
        // transition border-color .5s ease-out
        border-right thin solid #142d64
        border-bottom thin solid #142d64
    &-top-right
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-left thin solid white
        border-bottom thin solid white
      &--light
        // transition border-color .5s ease-out
        border-left thin solid #142d64
        border-bottom thin solid #142d64
    &-bottom-left
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-right thin solid white
        border-top thin solid white
      &--light
        // transition border-color .5s ease-out
        border-right thin solid #142d64
        border-top thin solid #142d64
    &-bottom-right
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-left thin solid white
        border-top thin solid white
      &--light
        // transition border-color .5s ease-out
        border-left thin solid #142d64
        border-top thin solid #142d64
</style>
