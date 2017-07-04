<template>
  <div>
    <slot name="title"></slot>
    <div class="inactive section__subtitle-container">
      <slot name="subtitle"></slot>
    </div>










    <h1 class="inactive section__title">
      {{ title }}<span v-if="title.span" v-text="title.span.content" :class="'section__subtitle section__subtitle--' + title.span.styling"></span>
    </h1>
    <!--<div :v-for="subtitle in subtitles"></div>-->
    <div class="inactive section__subtitle-container">
      <template v-for="subtitle in subtitles">
        <template v-if="!subtitle.count">
          <p :class="'section__subtitle section__subtitle--' + subtitle.styling">
            {{ subtitle.content }}<span v-if="subtitle.span" v-text="subtitle.span.content" :class="'section__subtitle section__subtitle--' + subtitle.span.styling"></span>
          </p>
        </template>
        <template v-if="subtitle.count">
          <p :class="'section__subtitle section__subtitle--' + subtitle.styling">          
            <span v-if="subtitle.count" v-text="subtitleBeforeCounting(subtitle)"></span>
            <!--<span v-if="subtitle.count" id="section__count-up" :numberCountingUp="numberCountingUp(subtitle)"></span>-->
            <!--<i-count-up
              v-if="subtitle.count"
              :start="0"
              :end="subtitle.count"
              :decimals="0"
              :duration="5.5"
              :options="options"
              :callback="callback"
              :id="`section__count--`"
            ></i-count-up>-->
            <span v-if="subtitle.count" v-text="subtitleAfterCounting(subtitle)"></span>
          </p>
        </template>
      </template>
    </div>
  </div>
</template>
<script>
import CountUp from 'countup.js/dist/countUp.min.js'
// import ICountUp from 'vue-countup-v2'

export default {
  props: ['title', 'subtitles'],
  components: {
    // ICountUp
  },
  data () {
    return {
      options: {
        useEasing: true,
        useGrouping: true,
        separator: ',',
        decimal: '.'
      }
    }
  },
  methods: {
    subtitleBeforeCounting (subtitle) {
      if (subtitle.count) {
        let subtitleBeforeCounting = subtitle.content.split(subtitle.count)[0]
        return subtitleBeforeCounting
      }
    },
    numberCountingUp (subtitle) {
      if (subtitle.count) {
        let options = {
          useEasing: true,
          useGrouping: true,
          separator: ',',
          decimal: '.'
        }
        let numAnim = new CountUp('section__count-up', 0, subtitle.count, 0, 2.5, options)
        numAnim.start()
      }
    },
    subtitleAfterCounting (subtitle) {
      if (subtitle.count) {
        let subtitleAfterCounting = subtitle.content.split(subtitle.count)[1]
        return subtitleAfterCounting
      }
    },
    callback: function (ins) {
      // ins.update(ins.endVal + 100)
      // let endVal = ins.endVal
      // console.log(ins)
      // ins.update(0)
      // ins.update(endVal)
      // ins.reset()
      // ins.start()
    }
  },
  mounted () {

  }
}
</script>
<style lang="stylus">
.section
  &__title
    color white
    font-size 55px
    // line-height 1.25
    font-weight 400
    margin 0
    &--active
      animation-duration .7s
      opacity 1

  &__subtitle-container
    &--active
      animation-duration .7s
      animation-delay .7s
      opacity 1

  &__subtitle
    color white
    font-size 25px
    line-height 1.5
    margin 1.5em 0
    text-shadow 2px 2px 8px black
    &--focus
      color yellow

  .inactive
    transition opacity .7s cubic-bezier(0,0,.2,1)
    opacity 0

@media (max-width: 1440px)
  .section
    &__title
      font-size 34px
    &__subtitle
      font-size 15.4px
</style>
