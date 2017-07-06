<template>
  <div id="app">
    <!--<header></header>-->
    <Header @toggleSidebar="toggleSidebar()" 
            :dataStoryId="currentPart.dataStoryId" 
            :dataStoryTitle="currentPart.dataStoryTitle" 
            :dataStoryIndex="currentPart.dataStoryIndex">
    </Header>

    <div class="ui right vertical sidebar labeled icon menu">
      <a class="item">
        <i class="home icon"></i>
        Home
      </a>
      <a class="item">
        <i class="block layout icon"></i>
        Topics
      </a>
      <a class="item">
        <i class="smile icon"></i>
        Friends
      </a>
    </div>
    <div class="pusher">
      <div id="fullpage">
        <template v-for="story in stories">
          <section v-for="(section, i) in story.sections"
                  :data-anchor="section.dataAnchor" 
                  :data-story-id="story.dataStoryId"
                  :data-story-title="story.dataStoryTitle" 
                  :data-story-index="story.dataStoryIndex"
                  :data-section-index="i"
                  class="section">
            <Description :class="'section__description section__description--' + section.descriptionModifier" 
                        :title="section.title" 
                        :subtitles="section.subtitles">
              <div slot="title" v-html="section.title"></div>
              <div slot="subtitle" v-html="section.subtitles"></div>
            </Description>
          </section>
        </template>
      </div>
    </div>
    <!--<div>
      <a id="jump" @click="jump">To P2S1</a>
    </div>-->
    <!--<div>
      <ul id="myMenu">
        <li data-menuanchor="11" class="active"><a href="#11">First section</a></li>
        <li data-menuanchor="21"><a href="#21">Second section</a></li>
      </ul>
    </div>-->
  </div>
</template>>

<script>
import Header from './components/Header'
import Description from './components/Description'
import $ from 'jquery'
import 'fullpage.js/dist/jquery.fullpage.js'
// import 'semantic-ui-sidebar/index.js'
$.fn.sidebar = require('semantic-ui-sidebar')

export default {
  name: 'app',
  components: {
    Header,
    Description
  },
  data () {
    return {
      currentPart: {
        dataStoryId: '',
        dataStoryTitle: '',
        dataStoryIndex: undefined
      },
      stories: {
        part1: {
          dataStoryId: 'what-is-going-on-in-KBlvd',
          dataStoryTitle: '凱道發生了什麼事？',
          dataStoryIndex: 1,
          sections: {
            section1: {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-1',
              descriptionModifier: 'left',
              title: `<h1 class="inactive section__title">到今天，卻仍有原住民在凱達格蘭大道上集結抗議</h1>`,
              subtitles: `<p class="section__subtitle section__subtitle">他們要求政府撤回「傳統領域劃設辦法」，歷經 3 次驅離，仍不離開，已經持續了 <span class="section__subtitle section--focus">103</span> 天。</p>`
            },
            section2: {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-2',
              descriptionModifier: 'left',
              title: `<h1 class="inactive section__title">蔡英文不是都道歉了嗎？</h1>`,
              subtitles: `<p class="section__subtitle section--focus">2016 年 8 月 1 日，總統蔡英文：</p>
                          <p class="section__subtitle section__subtitle">臺灣這塊土地，四百年前早有人居住。這些人原本過著自己的生活，有自己的語言、文化、習俗、生活領域。接著，在未經他們同意之下，這塊土地上來了另外一群人。」</p>
                          <p class="section__subtitle section__subtitle">「一個族群的成功，很有可能是建立在其他族群的苦難之上。除非我們不宣稱自己是一個公義的國家，否則這一段歷史必須要正視，真相必須說出來。然後，最重要的，政府必須為這段過去真誠反省，這就是我今天站在這裡的原因。」</p>`
            },
            section3: {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-3',
              descriptionModifier: 'center',
              title: '',
              subtitles: ''
            },
            section4: {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-4',
              descriptionModifier: 'center',
              title: `<h1 class="inactive section__title">為什麼需要有《傳統領域劃設辦法》？</h1>`,
              subtitles: `<p class="section__subtitle section__subtitle">雖然 2003 年《原基法》第 21 條賦予了原住民知情同意權，</p>
                        <p class="section__subtitle section__subtitle">但過去許多開發案，</p>
                        <p class="section__subtitle section__subtitle">開發方都主張「法律沒有明確定義原住民傳統領域範圍」，</p>
                        <p class="section__subtitle section__subtitle">所以原住民根本沒有實際實行過這個權利。</p>`
            }
          }
        },
        part2: {
          dataStoryId: 'what-is-traditional-realm',
          dataStoryTitle: '傳統領域是什麼？',
          dataStoryIndex: 2,
          sections: {
            section1: {
              dataAnchor: 'what-is-traditional-realm--section-1',
              descriptionModifier: 'left',
              title: `<h1 class="inactive section__title">傳統領域到底是什麼？</h1>`,
              subtitles: `<p class="section__subtitle section__subtitle">原住民傳統領域就是<span class="section__subtitle section--focus">原住民生活居住的空間，並不是私有權概念。</span></p>
                        <p class="section__subtitle section__subtitle">原民會也在此次《傳統領域劃設辦法》首次定義，傳統領域包括：原住民族傳統祭儀、祖靈聖地、部落及其獵區與耕墾區域。</p>`
            },
            section2: {
              dataAnchor: 'what-is-traditional-realm--section-2',
              descriptionModifier: 'left',
              title: '<h1 class="inactive section__title">根據原民會的調查，總共有 <span class="section__title section--focus">180</span> 萬公頃，占了整個台灣面積的 <span class="section__title section--focus">50%</span></h1>'
            },
            section3: {
              dataAnchor: 'what-is-traditional-realm--section-3',
              descriptionModifier: 'center',
              title: `<h1 class="inactive section__title section--gray">劃下去非原住民族都要去跳海了嗎？</h1>`,
              subtitles: `<p class="section__subtitle section--gray">只有一定程度以上的開發行為，</p>
                          <p class="section__subtitle section--gray">需要經過原住民部落同意（根據《諮商取得原住民部落同意參與辦法》）。</p>`
            },
            section4: {
              dataAnchor: 'what-is-traditional-realm--section-4',
              descriptionModifier: 'center',
              title: `<h1 class="inactive section__title section--gray">讓原住民畫了傳統領域</h1>
                      <h1 class="inactive section__title section--gray">會有什麼影響？</h1>`
            },
            section5: {
              dataAnchor: 'what-is-traditional-realm--section-5',
              descriptionModifier: 'left',
              title: `<h1 class="inactive section__title section--focus">實際權力</h1>`,
              subtitles: `<p class="section__subtitle section__subtitle">原住民可以在自己的傳統領域上行使《原基法》第 21 條賦予的權利：諮商、同意、參與權。</p>`
            },
            section6: {
              dataAnchor: 'what-is-traditional-realm--section-6',
              descriptionModifier: 'left',
              title: `<h1 class="inactive section__title section--focus">文化意義</h1>`,
              subtitles: `<p class="section__subtitle section__subtitle">原住民是這個島上最早的住民，但因為各個殖民政權讓原住民土地流失、生活空間被壓迫，導致很多傳統領域上根本沒有原住民。傳統領域從劃設過程就是一場尋根之旅，原住民的生活跟土地密不可分，保留的不只是生活空間，還有文化與歷史。</p>`
            }
          }
        },
        part3: {
          dataStoryId: 'the-meaning-of-traditional-realm',
          dataStoryTitle: '傳統領域對原住民的意義是什麼？',
          dataStoryIndex: 3,
          sections: {
            section1: {
              dataAnchor: 'the-meaning-of-traditional-realm--section-1',
              descriptionModifier: 'center',
              title: `<h1 class="inactive section__title">傳統領域對原住民的意義是什麼？</h1>`,
              subtitles: `<p class="section__subtitle section__subtitle">意義</p>
                          <p class="section__subtitle section__subtitle">意義</p>
                          <p class="section__subtitle section__subtitle">意義</p>`
            },
            section2: {
              dataAnchor: 'the-meaning-of-traditional-realm--section-2',
              descriptionModifier: 'left',
              title: `<h1 class="inactive section__title">來吉部落 鄒族農耕</h1>`,
              subtitles: `<p class="section__subtitle section__subtitle">流故一顯政頭、對買怕便中球何愛飯日事親師，小必是們，校基眾他……趣組一沒題！問合們應部身河後是是，本破演只訴觀整們兩滿、時來大寫破色！樣的態理象看輪的成育我不樣位的白查，生一帶？高近線的世治：不影計斷精車。</p>`
            }
          }
        },
        part4: {
          dataStoryId: 'the-issues-and-solutions-on-traditional-realm',
          dataStoryTitle: '爭議與解法',
          dataStoryIndex: 4,
          sections: {}
        }
      }
    }
  },
  methods: {
    jump () {
      $.fn.fullpage.silentMoveTo('21')
    },
    toggleSidebar () {
      $('.sidebar')
        // .sidebar({ context: $('#app') })
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    isStoryChange (dataAnchor, direction) {
      if (direction === 'up') {
        if (dataAnchor === 'what-is-traditional-realm--section-1' || dataAnchor === 'the-meaning-of-traditional-realm--section-1' || dataAnchor === 'the-issues-and-solutions-on-traditional-realm--section-1') {
          return true
        } else {
          return false
        }
      } else if (direction === 'down') {
        if (dataAnchor === 'what-is-going-on-in-KBlvd--section-4' || dataAnchor === 'what-is-traditional-realm--section-6' || dataAnchor === 'the-meaning-of-traditional-realm--section-2') {
          return true
        } else {
          return false
        }
      }
    },
    isBackgroundThemeChange (dataAnchor, direction) {
      if (direction === 'up') {
        if (dataAnchor === 'what-is-going-on-in-KBlvd--section-3' ||
            dataAnchor === 'what-is-going-on-in-KBlvd--section-4' ||
            dataAnchor === 'what-is-traditional-realm--section-3' ||
            dataAnchor === 'what-is-traditional-realm--section-5') {
          return true
        } else {
          return false
        }
      } else if (direction === 'down') {
        if (dataAnchor === 'what-is-going-on-in-KBlvd--section-2' ||
            dataAnchor === 'what-is-going-on-in-KBlvd--section-3' ||
            dataAnchor === 'what-is-traditional-realm--section-2' ||
            dataAnchor === 'what-is-traditional-realm--section-4') {
          return true
        } else {
          return false
        }
      }
    }
  },
  mounted () {
    let vm = this

    $(document).ready(() => {
      $('#fullpage').fullpage({
        // menu: '#myMenu',
        // anchors: ['11', '12', '13', '21', '22', '23'],
        // sectionsColor: ['#C63D0F', '#1BBC9B', '#7E8F7C', '#C63D0F', '#1BBC9B', '#7E8F7C'],
        animateAnchor: false,
        // navigation: true,
        // navigationPosition: 'right',
        afterLoad (anchorLink, index) {
          const loadedSection = $(this)
          const loadedSectionTitle = loadedSection.find('.section__title')
          const loadedSectionSubtitle = loadedSection.find('.section__subtitle-container')
          const loadedPartId = loadedSection.attr('data-story-id')
          const loadedPartTitle = loadedSection.attr('data-story-title')
          const loadedPartIndex = loadedSection.attr('data-story-index')

          vm.currentPart.dataStoryId = loadedPartId
          vm.currentPart.dataStoryTitle = loadedPartTitle
          vm.currentPart.dataStoryIndex = loadedPartIndex

          loadedSectionTitle.addClass('section__title--active')
          loadedSectionTitle.animateCss('fadeInUp')
          loadedSectionSubtitle.addClass('section__subtitle-container--active')
          loadedSectionSubtitle.animateCss('fadeInUp')
        },
        onLeave (index, nextIndex, direction) {
          const leavingSection = $(this)
          const leavingSectionTitle = leavingSection.find('.section__title')
          const leavingSectionSubtitle = leavingSection.find('.section__subtitle-container')
          const leavingSectionHeaderTitleContainer = $('.header__title')
          const leavingSectionId = leavingSection.attr('data-anchor')

          leavingSectionTitle.removeClass('section__title--active')
          leavingSectionTitle.addClass('inactive')
          leavingSectionSubtitle.removeClass('section__subtitle-container--active')
          leavingSectionSubtitle.addClass('inactive')

          if (vm.isBackgroundThemeChange(leavingSectionId, direction)) {
            if (leavingSectionId === 'what-is-going-on-in-KBlvd--section-2' ||
                leavingSectionId === 'what-is-going-on-in-KBlvd--section-4' ||
                leavingSectionId === 'what-is-traditional-realm--section-2' ||
                leavingSectionId === 'what-is-traditional-realm--section-5') {
              // dark to light
              // leavingSectionHeaderTitleContainer.animatefadeLight()
              leavingSectionHeaderTitleContainer.removeClass('white')
              leavingSectionHeaderTitleContainer.addClass('blue')
            } else {
              // light to dark
              // leavingSectionHeaderTitleContainer.animatefadeDark()
              leavingSectionHeaderTitleContainer.removeClass('blue')
              leavingSectionHeaderTitleContainer.addClass('white')
            }
          }

          if (vm.isStoryChange(leavingSectionId, direction)) {
            if (direction === 'up') {
              leavingSectionHeaderTitleContainer.animatefade('fadeOutDown')
            } else if (direction === 'down') {
              leavingSectionHeaderTitleContainer.animatefade('fadeOutUp')
            }
          }
        }
      })
    })

    //
    $.fn.extend({
      animateCss: function (animationName) {
        var animationEnd = 'webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend'
        $(this).removeClass('inactive')
        this.addClass('animated ' + animationName).one(animationEnd, function () {
          $(this).removeClass('animated ' + animationName)
        })
      },
      animatefade: function (animationName) {
        var animationEnd = 'webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend'
        $(this).removeClass('inactive')
        this.addClass('animated ' + animationName).one(animationEnd, function () {
          $(this).removeClass('animated ' + animationName)
          $(this).addClass('animated ' + 'fadeIn')
        })
      }
      // animatefadeLight: function () {
      //   var animationEnd = 'webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend'
      //   $(this).removeClass('inactive')
      //   $(this).addClass('short')
      //   this.addClass('animated ' + 'fadeOut').one(animationEnd, function () {
      //     $(this).removeClass('animated ' + 'fadeOut')
      //     $(this).removeClass('short')
      //     $(this).removeClass('white')
      //     $(this).addClass('blue')
      //     $(this).addClass('animated ' + 'fadeIn')
      //   })
      // },
      // animatefadeDark: function () {
      //   var animationEnd = 'webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend'
      //   $(this).removeClass('inactive')
      //   $(this).addClass('short')
      //   this.addClass('animated ' + 'fadeOut').one(animationEnd, function () {
      //     $(this).removeClass('animated ' + 'fadeOut')
      //     $(this).removeClass('short')
      //     $(this).removeClass('blue')
      //     $(this).addClass('white')
      //     $(this).addClass('animated ' + 'fadeIn')
      //   })
      // }
    })
  }
}
</script>

<style lang="stylus">
@import "../node_modules/fullpage.js/dist/jquery.fullpage.min.css"
@import "../node_modules/animate.css/animate.min.css"
@import "../node_modules/semantic-ui-sidebar/sidebar.min.css"
@import "../node_modules/semantic-ui-menu/menu.min.css"

.header
  position absolute
  top 0
  left 0
  width 100%
  height 120px
  // border 1px solid white
  z-index 100

.section
  background-size cover
  background-position center center
  // P1
  &[data-anchor="what-is-going-on-in-KBlvd--section-1"]
    background-image url("assets/what-is-going-on-in-KBlvd--section-1.jpg")
  &[data-anchor="what-is-going-on-in-KBlvd--section-2"]
    background-image url("assets/what-is-going-on-in-KBlvd--section-2.jpg")
  &[data-anchor="what-is-going-on-in-KBlvd--section-3"]
    background-color white
  &[data-anchor="what-is-going-on-in-KBlvd--section-4"]
    background-color #142d64
  // P2 
  &[data-anchor="what-is-traditional-realm--section-1"]
    background-image url("assets/what-is-traditional-realm--section-1.jpg")
  &[data-anchor="what-is-traditional-realm--section-5"]
    background-image url("assets/what-is-traditional-realm--section-5.jpg")
  &[data-anchor="what-is-traditional-realm--section-6"]
    background-image url("assets/what-is-traditional-realm--section-6.jpg")
  // P3
  &[data-anchor="the-meaning-of-traditional-realm--section-1"]
    background-color #142d64 
  &[data-anchor="the-meaning-of-traditional-realm--section-2"]
    background-image url("assets/the-meaning-of-traditional-realm--section-2.jpg")

  &__description
    width auto
    height 100%
    padding 100px 50px 0px 50px
    display flex
    flex-direction column
    &--left
      justify-content center
      align-items flex-start
      text-align left
      max-width 32%
      background linear-gradient(to left, rgba(0, 0, 0, 0), rgba(0, 0, 0, 1))
    &--center
      justify-content center
      align-items center
      text-align center
      max-width 100%

#app
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  // color: white;
  /*margin-top: 60px;*/
#jump
  position fixed
  top 10%
  left 10%

#myMenu
  position fixed
  top 50%
  left 10%
// div
  // font-size 2em
// .active
//   animation-duration .5s
//   opacity 1
// .inactive
//   transition opacity .7s cubic-bezier(0,0,.2,1)
//   opacity 0

</style>
