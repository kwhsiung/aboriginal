<template>
  <table :class="[`partition-nav`, `partition-nav--${theme}`]" @click="ga()">
    <tr>
      <td class="partition-nav__row">
        <div :class="setClass(1, 'top-left')">1</div>
      </td>
      <td class="partition-nav__row">
        <div :class="setClass(2, 'top-right')">2</div>
      </td>
    </tr>
    <tr>
      <td class="partition-nav__row">
        <div :class="setClass(3, 'bottom-left')">3</div>
      </td>
      <td class="partition-nav__row">
        <div :class="setClass(4, 'bottom-right')">4</div>
      </td>
    </tr>
  </table>
</template>

<script>
  export default {
    props: ['story', 'section', 'theme'],
    methods: {
      setClass (story, position) {
        if (story === this.story) {
          return [`partition-nav__content partition-nav__content--${this.theme} partition-nav__content-${position} partition-nav__content-${position}--${this.theme}`, `partition-nav__content--active--${this.theme}`]
        } else {
          return [`partition-nav__content partition-nav__content--${this.theme} partition-nav__content-${position} partition-nav__content-${position}--${this.theme}`]
        }
      },
      ga () {
        /* eslint-disable no-undef */
        ga('send', 'event', 'projects', 'click', 'nav open', { nonInteraction: true })
      }
    }
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
      // transition background-color .5s ease-out, color .5s ease-out
      // background-color white
      // color black
      &--dark
        transition background-color .5s ease-out, color .5s ease-out
        background-color white
        color black
      &--light
        transition background-color .5s ease-out, color .5s ease-out
        background-color #142d64
        color white
    &-top-left
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-right thin solid rgba(255, 255, 255, .7)
        border-bottom thin solid rgba(255, 255, 255, .7)
      &--light
        // transition border-color .5s ease-out
        border-right thin solid rgba(20, 45, 100, .7)
        border-bottom thin solid rgba(20, 45, 100, .7)
    &-top-right
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-left thin solid rgba(255, 255, 255, .7)
        border-bottom thin solid rgba(255, 255, 255, .7)
      &--light
        // transition border-color .5s ease-out
        border-left thin solid rgba(20, 45, 100, .7)
        border-bottom thin solid rgba(20, 45, 100, .7)
    &-bottom-left
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-right thin solid rgba(255, 255, 255, .7)
        border-top thin solid rgba(255, 255, 255, .7)
      &--light
        // transition border-color .5s ease-out
        border-right thin solid rgba(20, 45, 100, .7)
        border-top thin solid rgba(20, 45, 100, .7)
    &-bottom-right
      transition border-color .5s ease-out, background-color .5s ease-out, color .5s ease-out
      &--dark
        // transition border-color .5s ease-out
        border-left thin solid rgba(255, 255, 255, .7)
        border-top thin solid rgba(255, 255, 255, .7)
      &--light
        // transition border-color .5s ease-out
        border-left thin solid rgba(20, 45, 100, .7)
        border-top thin solid rgba(20, 45, 100, .7)
</style>
