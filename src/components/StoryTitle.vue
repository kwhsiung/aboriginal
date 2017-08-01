<template>
  <div :class="`header__title header__title--${theme}`" v-if="isVisible">
    {{ dataStoryTitle }}
  </div>
</template>
<script>
export default {
  props: [ 'dataAnchor', 'dataStoryTitle', 'theme' ],
  computed: {
    isVisible () {
      if (this.dataAnchor === 'landing-page' || this.dataAnchor === 'ending-page') {
        return false
      }
      if (this.dataAnchor === 'the-issues-and-solutions-on-traditional-indigenous-territories--section-5') {
        return false
      }
      if (this.dataAnchor === 'the-issues-and-solutions-on-traditional-indigenous-territories--section-6') {
        return false
      }
      return true
    }
  }
}
</script>

<style lang="stylus">

.header__title
  font-size 20px
  font-weight initial
  // text-shadow 2px 2px 8px black
  writing-mode vertical-lr
  position fixed
  top 90px
  right 55.5px
  z-index 101
  animation-duration .7s
  &--dark
    transition color .5s ease-out, text-shadow .5s ease-out
    color white
    text-shadow 2px 2px 8px black
  &--light
    transition color .5s ease-out, text-shadow .5s ease-out
    color #142d64
    text-shadow initial

@media (max-width: 1279px)
  .header__title
    font-size calc(20px/1.7)
    top 80px
    right 60px

@media (max-width: 1100px)
  .header__title
    display none

</style>

