<template>
  <div class="share right">
    <a class="share__icon share--toggle" @click="toggleShare()" :style="{ backgroundColor: `#142D64` }" >
      <!--<img :src="isOpen ? '../assets/close_white.png' : '../assets/share-white.png'" :alt="isOpen ? '關閉' : '開啟'">-->
      <img :src="isOpen ? getImgUrl('close_white') : getImgUrl('share-white')" :alt="isOpen ? '關閉' : '開啟'">
    </a>
    <a id="share-line" class="share__icon share__icon--list share--line" :class="[isOpen ? 'open' : '']" @click="shareLine()" target="_blank" href="http://line.naver.jp/R/msg/text/?有一群原住民族以街頭為家，為了現行的傳統領域劃設辦法阻擋了回「家」的路，抗議了超過百天。明明去年總統已向原住民族道歉，還成立特別的委員會處理族群轉型正義的問題，他們到底在抗議什麼⋯⋯？%0D%0Ahttps://www.mirrormedia.mg/projects/20170801aboriginal/"><img src="../assets/line_white_v2.png" alt="Line"></a>
    <a id="share-fb" class="share__icon share__icon--list share--fb" :class="[isOpen ? 'open' : '']" @click="shareFacebook()" target="_blank" href="https://www.facebook.com/share.php?u=https://www.mirrormedia.mg/projects/20170801aboriginal/" ><img src="../assets/facebook_white.png" alt="Facebook"></a>
    <a id="share-google" class="share__icon share__icon--list share--google" :class="[isOpen ? 'open' : '']" @click="shareGooglePlus()" target="_blank" href="https://plus.google.com/share?url=https://www.mirrormedia.mg/projects/20170801aboriginal/"><img src="../assets/google-plus.png" alt="Google Plus"></a>
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
      default: '#142D64'
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
      /* eslint-disable no-undef */
      ga('send', 'event', 'projects', 'click', 'share to gplus', { nonInteraction: false })
    },
    shareLine () {
      /* eslint-disable no-undef */
      ga('send', 'event', 'projects', 'click', 'share to line', { nonInteraction: false })
    },
    shareFacebook () {
      /* eslint-disable no-undef */
      ga('send', 'event', 'projects', 'click', 'share to fb', { nonInteraction: false })
    },
    toggleShare () {
      this.isOpen = !this.isOpen

      this.isOpen ? document.querySelector('.header__title-container').className += ' open' : document.querySelector('.header__title-container').classList.remove('open')
    },
    getImgUrl (pet) {
      var images = require.context('../assets/', false, /\.png$/)
      return images('./' + pet + '.png')
    }
  }
}
</script>

<style lang="stylus" scoped>

.share
  // display none
  // position fixed
  z-index 500
  // right 20px
  // bottom 20px
  width 38px
  height 38px
  cursor pointer
  &__icon
    display flex
    justify-content center
    align-items center
    position absolute
    top auto
    left auto
    width 38px
    height 38px
    border-radius 38px
    font-size 0
    transition transform .2s ease-out
    > img
      width auto
      height 20px
    &--list
      top auto
      left auto

  &--toggle
    width 38px
    height 38px
    border-radius 38px
    z-index 500

  &--line
    z-index 400
    background-color #00c300
    &.open
      transition-duration .19s
    > img
      width 20px
      height auto
      
  &--fb
    z-index 400
    background-color #3b5998
    &.open
      transition-duration .19s
    > img
      width 15px
      height auto
      
  &--google
    z-index 400
    background-color #dd4b39
    &.open
      transition-duration .19s
      
    > img
      width 20px
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
        transform translate3d(150px,0,0)
    .share--fb
      &.open
        transform translate3d(100px,0,0)
    .share--google
      &.open
        transform translate3d(50px,0,0)
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
// @media (min-width 600px)
//   .share
//     display inline-block

// @media only screen and (max-width: 736px) and (orientation: landscape)
//   .activity
//     &__share
//       display none

</style>
