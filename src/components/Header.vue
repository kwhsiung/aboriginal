<template>
  <header class="header">
    <div class="header__icon-container">
      <a href="http://www.mirrormedia.mg">
        <img class="header__logo" src="../assets/logo@2x.png" alt="鏡週刊 Mirror Media">
      </a>
      <Share></Share>
    </div>
    <div class="header__title-container">
      <div href="" class="header__title">{{ dataStoryIndex }}. {{ dataStoryTitle }}</div>
    </div>
    <div class="header__sidebar-toggler-container">
      <a @click="$emit('toggleSidebar')">
        <PartitionNav :index="dataStoryIndex"></PartitionNav>
      </a>
    </div>
  </header>
</template>

<script>
import Share from './Share'
import PartitionNav from './PartitionNav'

export default {
  props: ['dataStoryId', 'dataStoryTitle', 'dataStoryIndex'],
  components: {
    Share,
    PartitionNav
  },
  computed: {
  }
}
</script>
<style lang="stylus" scoped>
div
  display inline-block
.header
  box-sizing border-box
  display flex
  justify-content flex-start
  align-items center
  padding 0px 50px
  &__logo
    width 70px
    height 70px
  &__title-container
    font-size 25px
    margin-left 27px
    border-left 2px solid white
    line-height 70px
  &__icon-container
    display flex
    justify-content center
    align-items center
    // a
    //   display flex
    //   justify-content center
    //   align-items center
  &__title
    padding-left 19px
    text-decoration none
    color white
    &.delay
      animation-delay 2s
    &.white
      transition color .5s ease-out
      color white
    &.blue
      transition color .5s ease-out
      color #142d64
    &.short
      animation-duration .2s

  &__sidebar-toggler-container
    margin-left auto
</style>

