<template>
  <div class="header__title header__title--dark" v-if="isVisible">
    {{ dataStoryTitle }}
  </div>
</template>
<script>
export default {
  props: [ 'dataAnchor', 'dataStoryTitle' ],
  computed: {
    isVisible () {
      if (this.dataAnchor === 'landing-page') {
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
  font-size 25px
  font-weight 600
  // text-shadow 2px 2px 8px black
  writing-mode vertical-lr
  position fixed
  top 90px
  right 51.5px
  z-index 101
  animation-duration .7s
  // &.delay
  //   animation-delay 2s
  &--dark-landing-page
    transition color .5s ease-out
    color white
  &--dark
    transition color .5s ease-out, text-shadow .5s ease-out
    color white
    text-shadow 2px 2px 8px black
  &--light
    transition color .5s ease-out, text-shadow .5s ease-out
    color #142d64
    text-shadow initial

@media (max-width: 1280px)
  .header__title
    font-size calc(25px/1.5)
    top 80px
    right 58px

</style>

