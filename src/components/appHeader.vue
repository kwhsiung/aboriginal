<template>
  <header class="header">
    <div class="header__icon-container">
      <a href="http://www.mirrormedia.mg">
        <img class="header__logo" src="../assets/logo@2x.png" alt="鏡週刊 Mirror Media">
      </a>
      <Share></Share>
    </div>
    <div class="header__sidebar-toggler-container" v-if="dataStoryId !== 'landing-page'">
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
  props: ['dataStoryId', 'dataStoryIndex'],
  components: {
    Share,
    PartitionNav
  }
}
</script>
<style lang="stylus" scoped>
.header
  box-sizing border-box
  display flex
  justify-content flex-start
  align-items flex-end
  padding 0px 50px
  position absolute
  top 0
  left 0
  width 100%
  height 70px
  z-index 100
  &__logo
    width 40px
    height 40px
  &__icon-container
    display flex
    justify-content center
    align-items flex-end
    height 40px
    a[href="http://www.mirrormedia.mg"]
      display flex
      justify-content center
      align-items center
      margin-right 10px

  &__sidebar-toggler-container
    display inline-block
    margin-left auto

// @media (max-width: 1440px)
//   .header
//     height 80px
</style>

