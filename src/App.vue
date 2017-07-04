<template>
  <div id="app">
    <header></header>
    <div id="fullpage">
      <!--P1-->
      <section :data-anchor="part1.section1.dataAnchor" 
               :data-story-id="part1.dataStoryId" 
               class="section">
        <Description :class="'section__description section__description--' + part1.section1.descriptionModifier" 
                     :title="part1.section1.title" 
                     :subtitles="part1.section1.subtitles">
        </Description>
        <!--<div class="text animated fadeInUp">P1S1</div>
        <button class="btn animated pulse">this is a button</button>-->
      </section>
      <section :data-anchor="part1.section2.dataAnchor" 
               :data-story-id="part1.dataStoryId" 
               class="section">
        <Description :class="'section__description section__description--' + part1.section2.descriptionModifier" 
                     :title="part1.section2.title" 
                     :subtitles="part1.section2.subtitles">
        </Description>
      </section>
      <section :data-anchor="part1.section3.dataAnchor" 
               :data-story-id="part1.dataStoryId" 
               class="section">
        <div>P1S3</div>
      </section>
      <section :data-anchor="part1.section4.dataAnchor" 
               :data-story-id="part1.dataStoryId" 
               class="section">
        <Description :class="'section__description section__description--' + part1.section4.descriptionModifier" 
                     :title="part1.section4.title" 
                     :subtitles="part1.section4.subtitles">
        </Description>
      </section>

      <!--P2-->
      <section :data-anchor="part2.section1.dataAnchor" 
               :data-story-id="part2.dataStoryId" 
               class="section">
        <Description :class="'section__description section__description--' + part2.section1.descriptionModifier" 
                     :title="part2.section1.title" 
                     :subtitles="part2.section1.subtitles">
          <div slot="title" v-html="part2.section1.titless"></div>
          <div slot="subtitle" v-html="part2.section1.subtitless"></div>
        </Description>
      </section>
      <section id="p2s2" class="section">
        <div>P2S2</div>
      </section>
      <section id="p2s3" class="section">
        <div>P2S3</div>
      </section>
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
import Description from './components/Description'
import $ from 'jquery'
import 'fullpage.js/dist/jquery.fullpage.js'

export default {
  name: 'app',
  components: {
    Description
  },
  data () {
    return {
      part1: {
        dataStoryId: 'what-is-going-on-in-KBlvd',
        dataStoryTitle: '凱道發生了什麼事？',
        section1: {
          dataAnchor: 'what-is-going-on-in-KBlvd--section-1',
          descriptionModifier: 'left',
          title: '到今天，卻仍有原住民在凱達格蘭大道上集結抗議',
          subtitles: {
            para1: {
              content: '他們要求政府撤回「傳統領域劃設辦法」，歷經 3 次驅離，仍不離開，已經持續了 103 天。',
              count: 103,
              styling: 'normal'
            }
          }
        },
        section2: {
          dataAnchor: 'what-is-going-on-in-KBlvd--section-2',
          descriptionModifier: 'left',
          title: '蔡英文不是都道歉了嗎？',
          subtitles: {
            para1: {
              content: '2016 年 8 月 1 日，總統蔡英文：',
              styling: 'focus'
            },
            para2: {
              content: '「臺灣這塊土地，四百年前早有人居住。這些人原本過著自己的生活，有自己的語言、文化、習俗、生活領域。接著，在未經他們同意之下，這塊土地上來了另外一群人。」',
              styling: 'normal'
            },
            para3: {
              content: '「一個族群的成功，很有可能是建立在其他族群的苦難之上。除非我們不宣稱自己是一個公義的國家，否則這一段歷史必須要正視，真相必須說出來。然後，最重要的，政府必須為這段過去真誠反省，這就是我今天站在這裡的原因。」',
              styling: 'normal'
            }
          }
        },
        section3: {
          dataAnchor: 'what-is-going-on-in-KBlvd--section-3',
          descriptionModifier: 'left',
          title: '',
          subtitles: ''
        },
        section4: {
          dataAnchor: 'what-is-going-on-in-KBlvd--section-4',
          descriptionModifier: 'center',
          title: '為什麼需要有《傳統領域劃設辦法》？',
          subtitles: {
            para1: {
              content: '雖然 2003 年《原基法》第 21 條賦予了原住民知情同意權，',
              styling: 'normal'
            },
            para2: {
              content: '但過去許多開發案，',
              styling: 'normal'
            },
            para3: {
              content: '開發方都主張「法律沒有明確定義原住民傳統領域範圍」，',
              styling: 'normal'
            },
            para4: {
              content: '所以原住民根本沒有實際實行過這個權利。',
              styling: 'normal'
            }
          }
        }
      },
      part2: {
        dataStoryId: 'what-is-traditional-realm',
        dataStoryTitle: '傳統領域是什麼？',
        section1: {
          dataAnchor: 'what-is-traditional-realm--section-1',
          descriptionModifier: 'left',
          title: '傳統領域到底是什麼？',
          titless: `<h1 class="inactive section__title">傳統領域到底是什麼？</h1>`,
          subtitless: `<p class="section__subtitle section__subtitle--normal">原住民傳統領域就是<span class="section__subtitle section__subtitle--focus">原住民生活居住的空間，並不是私有權概念。</span></p>
                       <p class="section__subtitle section__subtitle--normal">原民會也在此次《傳統領域劃設辦法》首次定義，傳統領域包括：原住民族傳統祭儀、祖靈聖地、部落及其獵區與耕墾區域。</p>`,
          subtitles: {
            para1: {
              content: '原住民傳統領域就是',
              styling: 'normal',
              span: {
                content: '原住民生活居住的空間，並不是私有權概念。',
                styling: 'focus'
              }
            },
            // para2: {
            //   content: '原住民生活居住的空間，並不是私有權概念。',
            //   styling: 'focus-inline'
            // },
            para2: {
              content: '原民會也在此次《傳統領域劃設辦法》首次定義，傳統領域包括：原住民族傳統祭儀、祖靈聖地、部落及其獵區與耕墾區域。',
              styling: 'normal'
            }
          }
        },
        section2: {
          dataAnchor: 'what-is-traditional-realm--section-2',
          descriptionModifier: 'left',
          title: '根據原民會的調查，總共有 180 萬公頃，占了整個台灣面積的 50%'
          // subtitles: {
          //   para1: {
          //     content: '原住民傳統領域就是原住民生活居住的空間，並不是私有權概念。',
          //     styling: 'normal'
          //   },
          //   para2: {
          //     content: '原民會也在此次《傳統領域劃設辦法》首次定義，傳統領域包括：原住民族傳統祭儀、祖靈聖地、部落及其獵區與耕墾區域。',
          //     styling: 'normal'
          //   }
          // }
        },
        section3: {
          dataAnchor: 'what-is-traditional-realm--section-3',
          descriptionModifier: 'center',
          title: '劃下去非原住民族都要去跳海了嗎？',
          subtitles: {
            para1: {
              content: '只有一定程度以上的開發行為，',
              styling: 'normal'
            },
            para2: {
              content: '需要經過原住民部落同意（根據《諮商取得原住民部落同意參與辦法》）。',
              styling: 'normal'
            }
          }
        },
        section4: {
          dataAnchor: 'what-is-traditional-realm--section-4',
          descriptionModifier: 'center',
          title: '讓原住民畫了傳統領域會有什麼影響？'
          // subtitles: {
          //   para1: {
          //     content: '只有一定程度以上的開發行為，',
          //     styling: 'normal'
          //   },
          //   para2: {
          //     content: '需要經過原住民部落同意（根據《諮商取得原住民部落同意參與辦法》）。',
          //     styling: 'normal'
          //   }
          // }
        },
        section5: {
          dataAnchor: 'what-is-traditional-realm--section-5',
          descriptionModifier: 'left',
          title: '實際權力',
          subtitles: {
            para1: {
              content: '原住民可以在自己的傳統領域上行使《原基法》第 21 條賦予的權利：諮商、同意、參與權。',
              styling: 'normal'
            }
          }
        },
        section6: {
          dataAnchor: 'what-is-traditional-realm--section-6',
          descriptionModifier: 'left',
          title: '文化意義',
          subtitles: {
            para1: {
              content: '原住民可以在自己的傳統領域上行使《原基法》第 21 原住民是這個島上最早的住民，但因為各個殖民政權讓原住民土地流失、生活空間被壓迫，導致很多傳統領域上根本沒有原住民。傳統領域從劃設過程就是一場尋根之旅，原住民的生活跟土地密不可分，保留的不只是生活空間，還有文化與歷史。',
              styling: 'normal'
            }
          }
        }
      },
      part3: {
        dataStoryId: 'the-meaning-of-traditional-realm',
        dataStoryTitle: '傳統領域對原住民的意義是什麼？',
        section1: {
          dataAnchor: 'the-meaning-of-traditional-realm--section-1',
          descriptionModifier: 'center',
          title: '傳統領域對原住民的意義是什麼？'
        }
      },
      part4: {
        dataStoryId: 'the-issues-and-solutions-on-traditional-realm',
        dataStoryTitle: '爭議與解法'
      }
    }
  },
  methods: {
    jump () {
      $.fn.fullpage.silentMoveTo('21')
    }
  },
  mounted () {
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

          loadedSectionTitle.addClass('section__title--active')
          loadedSectionTitle.animateCss('fadeInUp')
          loadedSectionSubtitle.addClass('section__subtitle-container--active')
          loadedSectionSubtitle.animateCss('fadeInUp')
        },
        onLeave (index, nextIndex, direction) {
          const leavingSection = $(this)
          const leavingSectionTitle = leavingSection.find('.section__title')
          const leavingSectionSubtitle = leavingSection.find('.section__subtitle-container')

          leavingSectionTitle.removeClass('section__title--active')
          leavingSectionTitle.addClass('inactive')
          leavingSectionSubtitle.removeClass('section__subtitle-container--active')
          leavingSectionSubtitle.addClass('inactive')
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
      }
    })
  }
}
</script>

<style lang="stylus">
@import "../node_modules/fullpage.js/dist/jquery.fullpage.min.css"
@import "../node_modules/animate.css/animate.min.css"

header
  position absolute
  top 0
  left 0
  width 100%
  height 100px
  border 1px solid white
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
    background-color #132D64
  // P2 
  &[data-anchor="what-is-traditional-realm--section-1"]
    background-image url("assets/what-is-traditional-realm--section-1.jpg")

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
  text-align: center;
  color: white;
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
