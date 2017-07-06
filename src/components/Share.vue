<template>
  <div class="share right">
    <a class="share__icon share--toggle" @click="toggleShare()" :style="{ backgroundColor: `#356d9c` }" ><img :src="isOpen ? '../assets/close_white.png' : '../assets/share-white.png'" :alt="isOpen ? '關閉' : '開啟'"></a>
    <a id="share-line" class="share__icon share__icon--list share--line" :class="[isOpen ? 'open' : '']" @click="shareLine"><img src="../assets/line_white_v2.png" alt="Line"></a>
    <a id="share-fb" class="share__icon share__icon--list share--fb" :class="[isOpen ? 'open' : '']" @click="shareFacebook"><img src="../assets/facebook_white.png" alt="Facebook"></a>
    <a id="share-google" class="share__icon share__icon--list share--google" :class="[isOpen ? 'open' : '']" @click="shareGooglePlus"><img src="../assets/google-plus.png" alt="Google Plus"></a>
  </div>
</template>

<script>

// import { TOPIC_PROTEST_ID } from '../constants/index'
// import { shareGooglePlus, shareLine, shareFacebook } from '../util/comm'
// import _ from 'lodash'

export default {
  name: 'share',
  props: {
    direction: {
      type: String,
      default: 'right'
    },
    top: {
      default: 'auto'
    },
    right: {
      default: 'auto'
    },
    bottom: {
      default: 'auto'
    },
    left: {
      default: 'auto'
    },
    color: {
      default: '#356d9c'
    }
  },
  data () {
    return {
      isOpen: false
    }
  },
  computed: {
    isTimeline () {
      // return _.get(this.$store.state, [ 'route', 'params', 'topicId' ]) === TOPIC_PROTEST_ID
      return true
    },
    link () {
      // return _.get(this, [ 'sharePath' ], this.$store.state.route.path)
      return 'http://www.google.com'
    }
  },
  methods: {
    shareGooglePlus () {
      // shareGooglePlus({ route: this.link })
      console.log('shareGooglePlus')
    },
    shareLine () {
      // shareLine({
      //   route: this.link,
      //   title: document.querySelector('meta[property="og:title"]').getAttribute('content')
      // })
      console.log('shareLine')
    },
    shareFacebook () {
      // shareFacebook({ route: this.link })
      console.log('shareFacebook')
    },
    toggleShare () {
      this.isOpen = !this.isOpen
    }
  }
}
</script>

<style lang="stylus" scoped>

.share
  display none
  // position fixed
  z-index 500
  // right 20px
  // bottom 20px
  width 70px
  height 70px
  &__icon
    display flex
    justify-content center
    align-items center
    position absolute
    top auto
    left auto
    width 70px
    height 70px
    border-radius 70px
    font-size 0
    transition transform .2s ease-out
    > img
      width auto
      height 20px
    &--list
      top auto
      left auto

  &--toggle
    width 70px
    height 70px
    border-radius 70px
    z-index 500

  &--line
    z-index 400
    background-color #00c300
    &.open
      transition-duration .19s
    > img
      width 30px
      height auto
      
  &--fb
    z-index 400
    background-color #3b5998
    &.open
      transition-duration .19s
    > img
      width 20px
      height auto
      
  &--google
    z-index 400
    background-color #dd4b39
    &.open
      transition-duration .19s
      
    > img
      width 30px
      height auto
.activity
    &__share
      display block
      z-index 999

.share
  // &.top
  //   .share--line
  //     &.open
  //       transform translate3d(0,-150px,0)
  //   .share--fb
  //     &.open
  //       transform translate3d(0,-100px,0)
  //   .share--google
  //     &.open
  //       transform translate3d(0,-50px,0)
  &.right
    .share--line
      &.open
        transform translate3d(240px,0,0)
    .share--fb
      &.open
        transform translate3d(160px,0,0)
    .share--google
      &.open
        transform translate3d(80px,0,0)      
  // &.bottom
  //   .share--line
  //     &.open
  //       transform translate3d(0,150px,0)
  //   .share--fb
  //     &.open
  //       transform translate3d(0,100px,0)
  //   .share--google
  //     &.open
  //       transform translate3d(0,50px,0)
  // &.left
  //   .share--line
  //     &.open
  //       transform translate3d(-150px,0,0)
  //   .share--fb
  //     &.open
  //       transform translate3d(-100px,0,0)
  //   .share--google
  //     &.open
  //       transform translate3d(-50px,0,0)
@media (min-width 600px)
  .share
    display inline-block

@media only screen and (max-width: 736px) and (orientation: landscape)
  .activity
    &__share
      display none

</style>
