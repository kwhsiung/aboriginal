<template>
  <button id="down" @click="next">
    <icon name="angle-down" scale="4" class="angle-down"></icon>
  </button>
</template>
<script>
import $ from 'jquery'
import 'vue-awesome/icons/angle-down'
import Icon from 'vue-awesome/components/Icon'

export default {
  components: {
    Icon
  },
  methods: {
    next () {
      $.fn.fullpage.moveSectionDown()
    }
  },
  mounted () {
    window.setInterval(bounceArrow, 3000)

    function bounceArrow () {
      $('.angle-down').removeClass('animated slideInDown').delay(1000).queue(function (next) {
        $(this).addClass('animated slideInDown')
        next()
      })
    }
  }
}
</script>

<style lang="stylus">
#down
  position absolute
  bottom 50px
  left calc(50% - 40px)
  background-color transparent
  display flex
  justify-content center
  align-items center
  width 80px
  height 80px
  border-radius 80px
  padding 15px 20px
  /*display inline-block*/
  cursor pointer
  /*transition-property opacity, filter, transform*/
  /*transition-duration 0.15s*/
  /*transition-timing-function linear*/
  transition opacity 0.15s linear, filter 0.15s linear, transform 0.43s ease-out
  font inherit
  color white
  text-transform none
  /*background-color transparent*/
  border 1px solid white
  margin 0
  overflow hidden
  outline 0
  .angle-down
    animation-duration 1s
    
// For IPad
@media (max-width: 1100px)

// For Iphone 6+
// @media (max-width: 767px)

// For Iphone 6
// @media (max-width: 413px)
  #down
    transform scale(.7)
    bottom 0
// For IPhone 5/5S/SE
@media (max-width: 374px)
  #down
    transform scale(.5)
    bottom 0
</style>

