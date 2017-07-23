<template>
  <div id="app">
    <!--<header></header>-->
    <appHeader @toggleSidebar="toggleSidebar()" 
            :dataStoryId="currentStory.dataStoryId" 
            :dataStoryIndex="currentStory.dataStoryIndex"
            :theme="currentStory.theme">
    </appHeader>

     <StoryTitle :dataAnchor="currentSection.dataAnchor"
                 :dataStoryTitle="currentStory.dataStoryTitle">
    </StoryTitle>

    <div class="ui right vertical sidebar labeled icon menu" id="menu">
      <a :class="['item', {'active': currentStory.dataStoryIndex === 1}]" @click="moveTo('what-is-going-on-in-KBlvd--section-1', 1)">
        1. 凱道的原住民流浪記
      </a>
      <a :class="['item', {'active': currentStory.dataStoryIndex === 2}]" @click="moveTo('what-is-traditional-indigenous-territories--section-1', 2)">
        2. 什麼是傳統領域？
      </a>
      <a :class="['item', {'active': currentStory.dataStoryIndex === 3}]" @click="moveTo('the-meaning-of-traditional-indigenous-territories--section-1', 3)">
        3. 回不了家的原住民
      </a>
      <a :class="['item', {'active': currentStory.dataStoryIndex === 4}]" @click="moveTo('the-issues-and-solutions-on-traditional-indigenous-territories--section-1', 4)">
        4. 傳統領域對原住民來說是什麼？
      </a>
    </div>
    <div class="ui left sidebar vertical" id="readmore">
      <div v-html="currentSection.moreTitle"></div>
      <div v-html="currentSection.moreSubtitles"></div>
    </div>
    <div class="ui bottom sidebar vertical" id="issues-content">
      <div v-html="currentSection.currentIssueContent"></div>
    </div>
    <ButtonMore :theme="'hamburger--background-dark'" 
                :state="'closebtn'" 
                @closeissues="closeIssues"
                v-show="currentSection.showIssueContent" 
                style="position: absolute; bottom: 0; right: 0; margin-bottom: calc(5% - 30px); margin-right: calc(5% - 30px); z-index: 1000">
    </ButtonMore>

    <div class="container-more" v-show="currentSection.hasMore">
      <ButtonMore @more="more"></ButtonMore>
      <span class="container-more__caption">{{ currentSection.moreCaption }}</span>
    </div>
    <div class="pusher">
      <div id="fullpage">
        <template v-for="(story, storyIndex) in stories">
          <section v-for="(section, sectionIndex) in story.sections"
                  :data-anchor="section.dataAnchor" 
                  :data-story-id="story.dataStoryId"
                  :data-story-title="story.dataStoryTitle" 
                  :data-story-index="(+storyIndex)"
                  :data-story-total="stories.length"
                  :data-section-index="(+sectionIndex) + 1"
                  :data-section-total="story.sections.length"
                  :theme="section.theme"
                  class="section">
            <video poster="" id="bgvid" playsinline autoplay muted loop v-if="section.dataAnchor === 'landing-page'">
              <source src="http://inserthtml.com/demos/javascript/background-videos/flowers.mp4" type="video/mp4">
              <source src="http://inserthtml.com/demos/javascript/background-videos/video.webm" type="video/webm">
            </video> 
            <Description :class="'section__description section__description--' + section.descriptionModifier">
              <div slot="title" v-html="section.title"></div>
              <div slot="subtitle" v-html="section.subtitles" v-if="section.descriptionModifier !== 'space-around'"></div>
              <div slot="issues" class="space-around" v-if="section.descriptionModifier === 'space-around'">
                <template v-for="(issue, i) in section.issues">
                  <div class="space-around__container space-around__container--invisible" :style="'position: relative; animation-delay: .7s'">
                    <div v-html="issue.title"></div>
                    <div v-html="issue.subtitle"></div>
                    <ButtonMore :theme="'hamburger--background-dark'" style="position: absolute; bottom: 0" @readissues="readIssues(issue.content)"></ButtonMore>
                  </div>
                  <div class="vertical-line vertical-line--invisible" :style="'animation-delay: .7s'" v-if="i !== 2"></div>
                </template>
              </div>
            </Description>
            <ButtonDown v-if="section.dataAnchor === 'landing-page'"></ButtonDown>
            <!---->
            <img class="taiwan-line" src="./assets/taiwan_line.png" alt="" v-if="section.dataAnchor === 'what-is-traditional-indigenous-territories--section-2'">
            <img class="taiwan-color taiwan-color--invisible" src="./assets/taiwan_color.png" alt="" v-if="section.dataAnchor === 'what-is-traditional-indigenous-territories--section-2'">
          </section>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import appHeader from './components/appHeader'
import StoryTitle from './components/StoryTitle'
import Description from './components/Description'
import ButtonMore from './components/ButtonMore'
import ButtonDown from './components/ButtonDown'

import $ from 'jquery'
import 'fullpage.js/dist/jquery.fullpage.js'
import CountUp from 'countup.js/dist/countUp.min.js'
import _ from 'lodash'

// import 'semantic-ui-sidebar/index.js'
$.fn.sidebar = require('semantic-ui-sidebar')

export default {
  name: 'app',
  components: {
    appHeader,
    Description,
    ButtonMore,
    ButtonDown,
    StoryTitle
  },
  data () {
    return {
      currentStory: {
        dataStoryId: '',
        dataStoryTitle: '',
        dataStoryIndex: undefined,
        theme: ''
      },
      currentSection: {
        dataAnchor: '',
        hasMore: false,
        moreCaption: '',
        moreTitle: '',
        moreSubtitles: '',
        currentIssueContent: ``,
        showIssueContent: false
      },
      stories: [
        {
          dataStoryId: 'landing-page',
          dataStoryTitle: '空拍圖',
          sections: [
            {
              dataAnchor: 'landing-page',
              descriptionModifier: 'center',
              theme: 'dark-landing-page',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">沒有人是局外人？</h1>`,
              subtitles: `<p class="section__subtitle">原住民傳統領域劃設辦法，是台灣社會重新學習跟原住民相處的起點</p>`
            }
          ]
        },
        {
          dataStoryId: 'what-is-going-on-in-KBlvd',
          dataStoryTitle: '凱道的原住民流浪記',
          sections: [
            {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-1',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">沒有人是局外人？</h1>`,
              subtitles: `<p class="section__subtitle">這句最近很紅的口號，從電視新聞、金曲獎頒獎典禮、到在街頭抗議的人們身上都看得見。這句話，起源於 2 月 23 日，原住民歌手巴奈、那布和導演馬躍・比吼為了抗議《原住民族土地及部落範圍劃設辦法》，在凱達格蘭大道住了下來，歷經 3 次驅離，目前轉移陣地到台大醫院捷運站前，已經持續了 <span class="section__subtitle section--focus large" id="days">103</span> 天。</p>`
            },
            {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-2',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">蔡英文不是都道歉了嗎？</h1>`,
              subtitles: `<p class="section__subtitle section--gray">2016 年 8 月 1 日，總統蔡英文：「臺灣這塊土地，四百年前早有人居住。<br>這些人原本過著自己的生活，有自己的語言、文化、習俗、生活領域。<br>接著，在未經他們同意之下，這塊土地上來了另外一群人。」</p>
                          <p class="section__subtitle section--gray">「歷史的發展是，後來的這一群人，剝奪了原先這一群人的一切。<br>讓他們在最熟悉的土地上流離失所，<br>成為異鄉人，成為非主流，成為邊緣。 」</p>`
            },
            {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">原本，<br>原住民權益可能會在小英任內大躍進⋯⋯</h1>`,
              subtitles: `<ul>
                            <li class="section__subtitle marginless self-animate self-animate--invisible" style="animation-delay: .7s">首次以總統的高度向原住民道歉</li>
                            <li class="section__subtitle marginless self-animate self-animate--invisible" style="animation-delay: 1.4s">成立了原住民族歷史正義與轉型正義委員會，以總統為召集人</li>
                            <li class="section__subtitle marginless self-animate self-animate--invisible" style="animation-delay: 2.1s; animation-duration: 2s">公布《原住民族土地或部落範圍土地劃設辦法》，讓傳統領域終於有了實質效力</li>
                          </ul>
                          <p class="section__subtitle self-animate self-animate--invisible" style="animation-delay: 4.1s">⋯⋯等等，什麼是傳統領域？？？</p>`
            }
          ]
        },
        {
          dataStoryId: 'what-is-traditional-indigenous-territories',
          dataStoryTitle: '什麼是傳統領域？',
          sections: [
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-1',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">傳統領域是什麼？</h1>`,
              subtitles: `<p class="section__subtitle marginbottom-less">原住民族土地 ＝ 原住民傳統領域 ＋ 原住民保留地（原住民的私有地，由政府規劃發放）</p>
                          <p class="section__subtitle margintop-less">因為「原住民保留地」已經有相關法律處理，所以《原住民族土地或部落範圍土地劃設辦法》處理的是「原住民傳統領域」</p>
                          <p class="section__subtitle section--small">＊之後我們就簡稱《原住民族土地或部落範圍土地劃設辦法》為《劃設辦法》囉！</p>`
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-2',
              descriptionModifier: 'left-wide',
              theme: 'dark',
              hasMore: false,
              title: `<p class="section__title--invisible section__title">原住民傳統領域就是原住民生活居住的空間，並不是財產權、私有權概念。<br>原住民的生活跟土地有非常緊密的連結，<br>如打獵、採集、游耕等生活方式都是低密度使用。</p>`,
              subtitles: '<h1 class="section__subtitle">根據原民會的調查，<br>總共有 <span class="section__subtitle section--focus">180</span> 萬公頃，<br>占了整個台灣面積的 <span class="section__subtitle section--focus">50%</span></h1>'
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">那傳統領域可以拿來做什麼？</h1>`,
              subtitles: `<p class="section__subtitle">目前，《原基法》第 21 條授權，在傳統領域內一定程度開發行為，應該要向原住民諮商，並取得部落同意或參與，原住民也可以分享相關利益。簡稱「諮商同意權」。</p>`
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-4',
              descriptionModifier: 'center-wide',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">也只有特定行為需要經過原住民族同意</h1>`,
              subtitles: `<br>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 1.5s">開路</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 2.0s">商港、漁港、工業專用港、機場、直升機飛行場</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 2.5s">礦石採取</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 3.0s">蓄水、供水、抽水、引水工程及水庫、海水淡化廠、淨水處理廠</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 3.5s">觀光休閒旅館</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 4.0s">發電廠</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 4.5s">有毒物質措施放置</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 5.0s">火化場、公墓、骨灰存放設施</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 5.5s">軍事營區、軍港、海岸巡防營區、飛彈試射場、靶場、雷達站</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 6.0s">...</p>
                          <br>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 7.0s"">＊根據《諮商取得原住民部落同意參與辦法附件》</p>`
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-5',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">為什麼需要有《劃設辦法》？</h1>`,
              subtitles: `<p class="section__subtitle">雖然《原基法》第 21 條賦予了原住民諮商同意權，但過去許多開發案，財團、地方政府都曾主張「法律沒有明確定義原住民傳統領域範圍」，無視原住民的抗議。</p>
                          <p class="section__subtitle">從 2003 年立法以來，原住民從來沒有實際實行過這個權利。</p>
                          <p class="section__subtitle">所以 2015 年《原基法》修法，明訂主管機關原民會需要針對部落的<span class="section__subtitle section--focus">傳統領域範圍在哪裡、怎麼行使諮商同意權、權益受損如何補償</span>等實務上的細節另訂辦法，就是希望大家都不要再找藉口啦！</p>`
            }
          ]
        },
        {
          dataStoryId: 'the-meaning-of-traditional-indigenous-territories',
          dataStoryTitle: '回不了家的原住民',
          sections: [
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-1',
              descriptionModifier: 'center-wide',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">那為什麼原住民還在街上抗議不回家呢？</h1>`,
              subtitles: `<p class="section__subtitle">原民會在 2017 年 2 月 18 日按照 2015 年修法的結果，<br>
                          公佈了《劃設辦法》，<br>
                          <span class="section__subtitle section--focus">但可以劃的傳統領域卻只剩 80 萬公頃，</span><br>
                          <span class="section__subtitle section--focus">還排除私有地，只能在公有地上劃！</span></p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-2',
              descriptionModifier: 'left',
              theme: 'dark',
              title: `<h1 class="section__title--invisible section__title">消失的 100 萬公頃？</h1>`,
              subtitles: `<p class="section__subtitle"><mark class="mark--blue">Q</mark>：根據原民會過去的調查，傳統領域有 180 萬公頃。現在私有地不能劃，就變成 80 萬公頃嗎？</p>
                          <p class="section__subtitle"><mark class="mark--blue">原民會</mark>：私有地只有 20 幾萬公頃。先公告 80 萬公頃，是衡酌不是每個部落都有能力、意願劃設傳統領域，這些就需要由公家機關協助。根據我們能挹注的經費、與部落的能力，估計初步至少能先劃 80 萬公頃。傳統領域真正的範圍是多少，就要看部落，最後當然可能超過 80 萬。</p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              title: `<h1 class="section__title--invisible section__title">為什麼私有地不能劃？</h1>`,
              subtitles: `<p class="section__subtitle"><mark class="mark--blue">Q</mark>：傳統領域是原住民的文化與歷史，如果排除了私有地，文化就不完整了。</p>
                          <p class="section__subtitle"><mark class="mark--blue">原民會</mark>：《劃設辦法》既然來自《原基法》第 21 條的授權，劃出來的就是諮商同意權的範圍。雖然目前規範需經過部落同意的行為，大部分是公共建設，但裡面也有「蓋民宿」這種一般人會碰到的事啊，這就會涉及到私人財產權，有違背憲法的疑慮。</p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-4',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              moreCaption: '讓原住民保有傳統領域沒那麼可怕',
              moreTitle: `<h1>讓原住民保有傳統領域沒那麼可怕</h1>`,
              moreSubtitles: `<p>「如果你今天是地主，被原住民劃成傳統領域，你的權益會不會被影響？會不會受到限制？肯定會。」政大民族系副教授官大偉直言，「但限制合不合理，才是要討論的重點。」</p>
                              <p>事實上，在現行的法令中，私有土地本來就會受到各式各樣的限制。都市計畫、區域計畫、土地分區管制，也就是都會區的大樓能蓋多高、農地不能蓋工廠、水源保護區禁止污染事業等等，這些都是地主需要盡的合理社會義務，保障了別人，也保障了自己。</p>
                              <p>如果限制合理，自然不會有爭議；但假設限制超過地主應盡的社會義務，也有相關補償與救濟的機制。明明國家擬訂都市計畫也會碰到一樣的問題，相關法令也很完備，這次卻碰到原住民就轉彎。</p>
                              <p>「論先後順序，原住民傳統領域勢必跟後來出現的國家公有機關、私人產權有所重疊。但難道就無法達到雙贏嗎？」官大偉解釋，傳統領域權是一個很廣泛的概念，權利本來就有很多不同的層次，它可以是計畫高權，就像現在的國土計畫，是大方向規劃，規範的土地自然不分公私有；或者也可以是所有權、財產權、管理權、資源使用權、受益權等等，型態很多元。</p>
                              <p>官大偉舉《原基法》第 21 條規定諮商同意權與受益權為例，在傳統領域上的開發行為，需要經過原住民同意，還可以共享利益，直覺讓很多人對傳統領域的肯認打了退堂鼓。</p>
                              <p>但官大偉認為，這不一定要是對私人地主的剝奪，「我們在土地上開餐廳、營業，本來就要繳稅給國家。那假設稅收一部分變成原住民的發展基金，透過國家的重新分配，讓轉型正義可以完成，不是很好嗎？」</p>
                              <p>私有土地就不應該限制？他提到蘭嶼的屋子都是沿著山坡蓋，不管你的房子離海多遠，屋裡的人都能看到海。除了海洋對達悟族人的重要性，還包括觀察海象，看什麼時候可以出海。官大偉說，限制蓋房的高度或區位，這是對空間的管制，不是把你的土地沒收。</p>
                              <p>「這些權利都是傳統領域權的一種，不是全部土地都要收回來才是實踐傳統領域的權利。但這些都需要細細地去談，而不是像現在的做法，碰到困難就轉彎。」官大偉說。</p>`,
              title: `<h1 class="section__title--invisible section__title">關鍵：原住民傳統領域只有諮商同意權一種功能嗎？</h1>`,
              subtitles: `<p class="section__subtitle">原民會土地管理處傳統領域科吳科長指出，公部門只能依法論法，既然《劃設辦法》是依據《原基法》第 21 條訂定，劃設出來的傳統領域也只能行使諮商同意權。</p>
                          <p class="section__subtitle">但「原住民傳統領域」在未來的生生世世難道就不會出現在其他法律裡了嗎？</p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-5',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              moreCaption: '現行劃設辦法會造成立即的侵害！',
              moreTitle: `<h1>現行劃設辦法會造成立即的侵害！</h1>`,
              moreSubtitles: `<p>「國土規劃涉及原住民族之土地，應尊重及保存其傳統文化、領域及智慧，並建立互利共榮機制。」這是去年通過的《國土計畫法》第六條第九項，針對國土規劃的基本規則，提及對於原住民權利的保障。</p>
                              <p>原住民族對土地有很多「移動性」的使用方式，游耕、游牧、狩獵等等，現行法規中卻沒有一個分區可供使用。政大民族系副教授官大偉解釋，台灣的土地使用法規是從大陸的生態系統發展出來，土地很厚，長期從事同一種行為也不致枯竭；但台灣屬於新生島嶼，土壤很薄，在同一塊土地上混用反而更能維護地力。</p>
                              <p>倡議了許久，終能在《國土計畫法》中找到邁向自治的契機。《國土計畫法》將重新盤點台灣的土地使用，其中「原住民特定區域計畫」就能讓原住民有用自己文化擬定土地使用的方式，現在內政部區委會也正以鎮西堡部落為先行研究對象，7 月 17 日才去部落現勘。</p>
                              <p>不過，翻出《國土計畫法》的相關條文，都是用「原住民族土地」這樣的字眼。《原基法》定義原住民族土地包含原住民保留地、原住民族傳統領域，而現在如果將傳統領域範圍排除私有地，未來提及「原住民族土地」的原住民族權利全部都會被限縮在公有地上了。</p>`,
              title: `<h1 class="section__title--invisible section__title">劃設辦法只是起步，</h1>
                      <h1 class="section__title--invisible section__title">不應該成為傳統領域的定義！</h1>`,
              subtitles: `<p class="section__subtitle">按照總統蔡英文的說法，<br>是因為原住民若在私人土地上行使諮商同意權，會有侵害私人財產權的疑慮，<br>才讓傳統領域排除私有地，希望大家不要一直吵吵鬧鬧造成政策無法前進。</p>
                          <p class="section__subtitle">但，這等於是將原住民傳統領域的範圍限縮在公有地上。</p>
                          <p class="section__subtitle">雖然依據現行的法律，原住民在傳統領域上只能行使知情同意權，但不代表未來都是如此。<br>例如去年才通過的《國土計畫法》，<br>原住民可以在原住民族土地上實施特定區域計畫，是原住民邁向自治的重要關鍵。</p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-6',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">政府打算這樣做</h1>`,
              subtitles: `<ul class="li--inside">
                            <span class="x x--invisible" id="x1"></span>
                            <li class="section__subtitle section--gray self-animate self-animate--invisible" style="animation-delay: .7s">由於有侵害個人財產權的疑慮，劃設辦法分階段實施，現在先處理公有土地</li>
                            <p class="section__subtitle section--red self-animate self-animate--invisible" style="animation-delay: 2.1s">這樣就將原住民傳統領域限縮在公有土地上了</p>
                          </ul>
                          <ul class="li--inside">
                            <span class="x x--invisible" id="x2"></span>
                            <li class="section__subtitle section--gray self-animate self-animate--invisible" style="animation-delay: 2.8s">私有土地的部分未來將推動《原住民族土地及海域法》處理</li>
                            <p class="section__subtitle section--red self-animate self-animate--invisible" style="animation-delay: 4.2s">連低強度的知情同意權都引起這麼大的爭議，<br>土海法處理的是所有權返還、資源共管這種更進一步的權利，<br>非常困難</p>
                          </ul>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-7',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">民間建議這樣做</h1>`,
              subtitles: `<ul class="li--inside">
                            <li class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: .7s">退回這個極具爭議的劃設辦法</li>
                            <li class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: 1.4s">既然目前只擔心侵害個人財產權，<br>那就將「哪些行為需經過原住民諮商同意」再定義清楚，<br>修改《諮商取得原住民部落同意參與辦法附件》就好了</li>
                            <li class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: 2.1s">將劃設辦法中的文字更明確地修改為「劃設公有地中的原住民族傳統領域」，<br>而非「原住民族傳統領域排除私有地」</li>
                          </ul class="li--inside">`
            }
          ]
        },
        {
          dataStoryId: 'the-issues-and-solutions-on-traditional-indigenous-territories',
          dataStoryTitle: '傳統領域對原住民來說是什麼？',
          sections: [
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-1',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              moreCaption: '看台東都蘭部落的故事',
              moreTitle: `<h1>台東都蘭部落：「傳統領域就是家」</h1>`,
              moreSubtitles: `<p>一個下著大雨的夜晚，我們在都蘭海邊盯著兩個光點。一個多小時後，先上岸的是比較資深的一南（Yinam），手裡的籃子有海膽、海螺跟一條魚。他用的是漁槍，因為這幾天下雨，水不夠清澈，造成視線不佳，並不是捕魚的好時機。</p>
                              <p>但在等待一南的過程中，也有兩個阿美族人上岸來。兩個人用的是漁網，捕到約莫六、七隻魚，我們一邊驚嘆，他們只淡淡地說「一天就吃完啦」。</p>
                              <div class="video-container"><iframe width="560" height="315" src="https://www.youtube.com/embed/2Opw2tEn9sI" frameborder="0" allowfullscreen></iframe></div>
                              <p>台東縣都蘭部落在 2 月 28 日自主宣告傳統領域，總共 74 個地名，連海洋也算在內。部落靠海，親近海洋被認為是理所當然的活動，如果年輕人很久沒去、或明明工作放假還待在家裡不下海，會被視為懶惰。</p>
                              <p>第一次「捕魚」，是國小三年級中午放學下課跟表弟去海邊幫爸爸收網。一南記得當天的浪很大，「我帶著游泳圈就下去了，邊哭邊收耶！風很大很害怕，水母又多，以前又沒有潛水衣，穿著內褲就下去了。上岸的時候覺得命還在算是撿到的吧！」</p>
                              <p>真正潛水打魚，是一南當兵後的事了。他越級三層，來到哥哥們的年齡階層，跟著哥哥一起下水打魚，學習使用魚槍、看海況等各種捕魚的知識。三年之後，他回到自己的年齡階層，將知識與經驗分享給同儕。</p>
                              <figure>
                                <div class="readmore__img" id="dolan1"></div>
                                <figcaption>一南（左）與年齡階層較年輕的阿倫（右）一同入海捕魚。</figcaption>
                              </figure>
                              <figure>
                                <div class="readmore__img" id="dolan2"></div>
                                <figcaption>兩人在上岸後分享捕到的漁獲。由於海水能見度不佳，多是撿拾的貝類。</figcaption>
                              </figure>
                              <p>在一旁的台東大學助理教授蔡政良遞上啤酒。蔡政良原為客家人，後來擬親成為阿美族，現在是阿度蘭阿美斯文化協進會的總幹事。他與一南屬於同一個年齡階級，目前是部落的「策動組」，是組織活動運轉的核心。</p>
                              <p>蔡政良形容，在得知要接下策動組重責大任的前幾年，年齡階層中的同儕都很緊張。但不約而同，大家都會約去海邊打魚，像個心照不宣的默契，「在海邊打魚的時候，生命是交給彼此的。我們曾經出事過，也把彼此救回來。」</p>
                              <p>蔡政良說：「過去沒有承擔責任、權力的時候，不會感受到。但一起去海邊打魚，就讓我們階層團結起來。」海洋陪著部落男孩長大，從部落的文化傳承、生活所需，到成為一個男人需負擔的責任，它全讓族人來懷裡索取。</p>
                              <p>正是因為海洋與生活的密切結合，都蘭部落對於欲私有化海灘的開發案抗爭不遺餘力，如知名的反美麗灣運動。儘管成功擋下，比美麗灣大數倍的杉原灣度假村，仍在今年初通過了環境差異分析，顯示財團對東海岸的野心從未停息。</p>
                              <p>都蘭部落自主宣告的傳統領域不分公私有，但「權利」卻公私有別。如「須諮詢部落取得同意」後才能做的事，是不分公私有土地內的「經濟性開發行為」及「牽涉倫理的人為活動」；而涉及部落「共享利益」，卻僅限於「公產機關在傳統領域上的經濟利益」。</p>
                              <p>蔡政良提及，這就是傳統領域跟私人財產權無關的證明，「諮商同意權就是你來我家附近當鄰居，當然要打聲招呼，部落的人都相當清楚，要有一定規模的開發，如超過一公頃、或是蓋下去會引發土石流、影響飲用水等等，才要諮詢。否則三天兩頭就召開部落會議，哪有那麼多時間？」</p>
                              <p>但現行排除私有地的傳統領域劃設辦法，讓都蘭部落面臨了立即的威脅。去年開始，欲在知名景點水往上流附近興建 500 個房間規模的 ATT 度假村，就全部是私有地。雖然興建度假村仍須經過環評，但部落族人對於一個在「家」附近的開發案居然毫無置喙空間。</p>
                              <p>「這幾年面對東海岸的開發案，部落老人家有一種很深的焦慮，面對土地慢慢流失，拿不回來。但我們也很清楚，按照現在的劃設辦法，土地的主權不會有改變。只有諮商同意權、利益共享而已。」蔡政良說，「但在心理層面上，有了諮商同意權，是認定原住民是土地的主人。還是比較正向的發展。」</p>
                              <p>訪問過程突然雷聲大作，一南不自覺地抖了一下，斜眼看著天空苦笑，說「趕快跑呦」，要是知道會打雷，絕對不下海。他們收拾東西，在愈下愈大的雨裡準備去蔡政良家喝魚湯了。我趕緊追問一南，海是什麼呢？</p>
                              <p>「海啊，就是不管我再怎麼累，一看到海，整個心情就好輕鬆，下去游一游，就不累了。在家裡看電視還會打瞌睡，還會被老婆唸啊，但一跳進海裡，什麼煩惱都沒有了。跟朋友們也是有空就約在海邊聯絡感情。」一南說，「大海不只幫助阿美族的生活，包括感情聯繫、儀式祭典都在海邊，讓大家可以聚在一起。</p>
                              <p>在阿美族的族語裡，並沒有「傳統領域」這個字。對他們來說，傳統領域就是「家」。</p>
                              <figure class="captionless">
                                <div class="readmore__img" id="dolan3"></div>
                              </figure>
                              <figure class="captionless">
                                <div class="readmore__img" id="dolan4"></div>
                              </figure>
                              <figure class="captionless">
                                <div class="readmore__img" id="dolan5"></div>
                              </figure>`,
              title: `<h1 class="section__title--invisible section__title">台東都蘭部落：<br>「傳統領域就是家」</h1>`,
              subtitles: `<p class="section__subtitle">倚海而居的台東縣阿美族都蘭部落，在今年 2 月自主宣告了傳統領域，連海洋也包含在內。傳統領域不分公私有地，但在部落的管制規則中，權利卻公私有別，僅公有機關的開發案需共享利益，私有地不用；但私有地具規模的開發仍須經部落同意，他們認為這是一種「當鄰居前的告知」。東海岸面臨的開發威脅從未少過，現在又有一個完全是私有地的度假村要來闖關⋯⋯</p>`
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-2',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              moreCaption: '看台東卡大地不部落的故事',
              moreTitle: `<h1>台東卡大地不部落</h1>`,
              moreSubtitles: `<p>nope</p>`,
              title: `<h1 class="section__title--invisible section__title">台東卡大地不部落</h1>`,
              subtitles: `<p class="section__subtitle">nope</p>`// TODOs: TBA
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              moreCaption: '看阿里山來吉部落的故事',
              moreTitle: `<h1>阿里山來吉部落：人與自然的戀曲</h1>`,
              moreSubtitles: `<p>來到阿里山來吉部落，沿路可見一個個鮮豔的山豬石雕，成為部落鮮明的標誌，還有人稱這裡是「山豬公園」、「山豬部落」。相傳當年鄒族獵人為了追逐山豬，來到了一塊肥沃的土地，決定在此定居，但這些山豬石雕可不單單只是因為這個故事。</p>
                              <p>這些美麗的石雕藝術，最早出自藝術家不舞的手，山豬幾乎成為她創作的招牌。不舞並不是在部落長大的孩子，當年只是因為在部落經營小吃店的母親希望她回來幫忙，她就此留了下來。</p>
                              <p>「其實媽媽蠻特別的，一般的父母不是都希望孩子能在外面好好發展？好好工作、賺錢，但我媽就會叫我回山上幫她的忙。」不舞提到，她從小就對畫畫有興趣，回到部落，或許也能做自己想做的事，「我看山上的人都胖胖的啊！雖然可能賺不到什麼錢，但好像也過得不錯。」她笑說。</p>
                              <p>回到部落以後，她從跟老人家的閒聊中開始瞭解鄒族的文化故事，對此深深著迷，也變成了她創作的養分。</p>
                              <p>如鄒族的聖山塔山，「我覺得塔山給我的感覺非常美好、溫暖，看到這座山就好像有依靠」或是孤挺花，「孤挺花是族人喜歡配戴的頭花，我外婆說，部落有很多遺跡，是過去的人留下的生活證據，但現在都消失了。不過你可以找到孤挺花，是以前的族人種的，它很美，而且不必太刻意照顧也能開出美麗的花朵」，不舞信手拈來都是故事，也象徵著族人的生活方式。</p>
                              <p>那山豬呢？不舞曾經有一頭寵物山豬，是媽媽送她的。她還曾經跟山豬約定，要牠變成全世界最老的山豬，但有次她回家，卻發現山豬被媽媽殺掉了。後來不舞夢到了牠，從此開始做跟山豬有關的創作，並從中得到療癒。</p>
                              <p>「一開始做山豬石雕，族人會有很多問號，覺得這能代表部落嗎？但當外面的遊客看到這樣的作品，好像蠻開心的，也會跟它們合照，分享出去。」不舞提到，後來這也變成八八風災後部落重新站起來的力量，利用「山豬部落」來對外行銷，她也把工藝技術帶進來，現在在部落裡的石雕，有些出自其他族人的手。</p>
                              <p>不舞不只是藝術家，還是原住民中第一個拿到有機茶認證的農夫。大約 15 年前，不舞說當時也不是很瞭解「有機」的概念，只覺得有機對人是友善的，按照原住民的生活方式，就是要走有機。</p>
                              <p>我們來到不舞的茶園，茶樹長得很高，還高矮不一，錯落著雜草，幾乎把人都淹沒。「做有機很孤獨，也很沒有效率，但最起碼，我們可以喝到健康無毒的茶。」不舞說。走了十幾年，在外人看來相當辛苦的事，她們僅靠著簡單的信念就走過來。</p>
                              <p>「其實從外人看來，原住民的生活方式很沒有經濟效益啊。但過去幾百年，我們就是這樣生活的，不想著賺錢，就能自給自足。」不舞提到家族的歷史，阿古雅那家族的傳統領域就在現在的阿里山遊樂區，甚至「阿里山」之所以這樣命名，就是來自家族內的厲害首領阿巴里。</p>
                              <p>不舞提到，以前鄒族人管理傳統領域，大家都維護得很好，因為這是屬於你的獵場，不能讓資源枯竭。不管是河流裡的魚、山裡的獵物、哪些土地可以種田，如果數量開始減少、地力有枯竭的跡象，就像換另外一區，大家會跑來跑去。很多行為背後都有智慧，就像原住民會砍掉一些樹木座小範圍的耕種，是為了讓光線透進來，鳥類會變多，生物的多樣性就會增加。</p>
                              <p>「以前的林相，大樹都不能砍，所以才有破千年的檜木林。因為萬物都有靈，你也可以跟森林對話，不過以前的人走到森林裡是不講話的，因為會吵到神靈。在森林裡唯一的聲音就是唱歌，或是敲擊樂器，用這樣的方式來跟神靈說話。」不舞說，「以前的人面對自然環境，是彼此尊重」。</p>
                              <p>但他提到，土地孕育了鄒族人，在這四百年遇到荷蘭人、日本人、到現在的國民政府，雖然四百年相比於千年的歷史很短，但在這麼短的時間內，島嶼內的很多事情卻被消耗了，文化、語言都快要消失。</p>
                              <p>「像現在的阿里山神木，它們過去跟原住民生活了很久很久，現在很多卻只剩下檜木頭。過去的美好林相，現在都變成遺跡了。」不舞提到，甚至後來有一陣子台灣得靠賣樹維生，在族人看來是非常傷心的事。</p>
                              <p>「阿里山有個姊妹潭，我外公的爸爸他的工寮就在那裡。日治時代的時候，日本人有畫界碑，這個區域妳要在這裡做任何工作，你要跟原住民租。」不舞說，後來國民政府來台，外公不想租了，就收回來做了一個歌舞團。後來發生了一場火災，國家做了暫時的安置，隔一兩年，大家又回到自己的部落。</p>
                              <p>但政府後來為了興建阿里山森林遊樂區，「當時國民政府說你有在使用的，跟國家登記，剩下全部是國家的。」不舞無奈地說，「但以前的人沒有說這是自己的，我們都會說那是家族的。後來阿里山森林遊樂區都是林務局在管，現在是看不到原住民的。」</p>
                              <p>她提到，國家就蓋了一座門來收門票。「我們有時候走到（遊樂區），不管是打獵、還是⋯⋯，也都會覺得，到底（我們）是主人還是客人？到那邊我們還是要報備啊，我們是哪一村的，因為我們是原住民才不收門票。」不舞苦笑，述說著矛盾的心情。</p>
                              <p>在原住民傳統領域劃設辦法公布之後，雖然爭議四起，原民會卻也同時強調目前已有 139 個部落、 3 個民族申請劃設，顯見這是原住民族長期共同期待的目標。阿里山鄒族也在也在「搶先申請」的名單之列。</p>
                              <p>在鄉公所參與過傳統領域調查專案的鄒族族人湯文賢表示，調查起源於原民會的先期示範計畫，他只負責調查，後來就離開，是鄉公所依照示範計畫的調查結果去申請的，他自己並不贊成現在的劃設辦法排除私有地，也提到在調查的時候根本不可能排除私有地。</p>
                              <p>但除此之外，若鄉公所沒有更改調查結果，向原民會提報申請的鄒族傳統領域就包含阿里山森林遊樂區。未來，是否真有機會讓部落享有諮商同意權、甚至森林遊樂區的利益共享？就看新政府要如何落實口中的轉型正義了。</p>
                              <p>湯文賢也苦笑著說，就算調查、確立了傳統領域，部落內對這件事並沒有太熱情，「族人很清楚，就算確認了傳統領域的範圍，政府也沒有要還給我們，「我們只能不斷地說明為什麼這件事這麼重要，至少是一個起點」。</p>
                              <p>提到對傳統領域權的期待，不舞提到了她最愛的故事——山豬王與少女，故事述說山豬王與鄒族少女相戀的故事，雖然最後以悲劇終結，卻也彰顯過去大自然與人類的關係非常緊密。「希望國家能把土地還給真正珍愛土地的人，原住民就有機會跟大自然再度譜出戀曲吧」，她笑著說。</p>`,
              title: `<h1 class="section__title--invisible section__title">阿里山來吉部落：<br>人與自然的戀曲</h1>`,
              subtitles: `<p class="section__subtitle">來到阿里山來吉部落，沿路可見一個個鮮豔的山豬石雕，成為部落鮮明的標誌，還有人稱這裡是「山豬公園」、「山豬部落」。相傳當年鄒族獵人為了追逐山豬，來到了一塊肥沃的土地，決定在此定居，但這些山豬石雕可不單單只是因為這個故事。</p>`// TODOs: TBA
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-4',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              moreCaption: '看新竹司馬庫斯部落的故事',
              moreTitle: `<h1>新竹司馬庫斯部落</h1>`,
              moreSubtitles: `<p>nope</p>`,
              title: `<h1 class="section__title--invisible section__title">新竹司馬庫斯部落</h1>`,
              subtitles: `<p class="section__subtitle">nope</p>`// TODOs: TBA
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-5',
              descriptionModifier: 'space-around',
              theme: 'dark',
              issues: [
                {
                  title: `<h1 class="section__issue-title">「誰」可以畫傳統領域？</h1>`,
                  subtitle: `<p class="section__subtitle">要完整實踐原住民的知情同意權，解決了「在哪裡劃」、「在什麼情形下可以行使權利」的問題以後，還有「誰可以劃」的問題。劃設傳統領域的權利主體是誰？是什麼樣的組織？</p>`,
                  content: `<p>雖然原民會已有規劃要推動「部落公法人」機制，但目前對這個組織的想像卻還很空泛。</p>
                            <p>原住民族政策協進會執行長陳旻園就指出，從傳統領域的劃設、到處理所有權的《土海法》，過去推動這麼多年一直碰到一個問題就是：土地要畫給誰？誰有權利主張？</p>
                            <p>「當然，集體意識強的族群比較會講話，但對大部分的族群跟部落來說，內部的討論機制都已經被現在的民意機關、組織架空了。」陳旻園指出，部落裡可能有民族議會、教會、協會等有代表性的組織，但在法律上只有行政機構跟民意代表有法源依據，這是為什麼目前看到的部落代表都是後者。</p>
                            <p>陳旻園指出，主體需要定義清楚，部落也需要政府協助培力。他舉劃設辦法為例，要先組一個劃設團隊、再提出範圍計畫、送到鄉公所、核定完之後再送到縣市政府⋯⋯，「程序之複雜，就像過去保留地的增擴編，持續了 20 幾年，還有好幾萬件卡在公部門過不去，這也會造成族人的消極，反正又過不了。」</p>
                            <p>「最好的方式，就是主動協助。這不就是主管機關要做的嗎？今天已經有這麼多部落自主宣告，那原民會做了什麼？你要去鼓勵、去幫忙，其他部落看到成效出來，就會覺得我也可以試試看。」陳旻園感嘆，本應該是照顧族人的部會，反而設下這麼高的門檻，像在打小孩給別人看。</p>
                            <p>但陳旻園也提醒，原住民也應該準備好，「如果部落沒有主體，還要等待政府的輔佐，等待政府劃一塊傳統領域給你，那跟過去的福利政策有什麼不同？對於社會大眾來說，也會面臨一個問題，原住民憑什麼？永遠就陷在這樣的爭議裡。」</p>`
                },
                {
                  title: `<h1 class="section__issue-title">《土海法》會是萬靈丹嗎？</h1>`,
                  subtitle: `<p class="section__subtitle">按照總統蔡英文的指示，傳統領域的劃設要「分階段」實行，私有地要透過《原住民土地及海域法》來處理。過去三進三出立法院的《土海法》，這次過得了關嗎？</p>`,
                  content: `<p>私有地被排除在傳統領域之外，除了諮商同意權行使範圍無法完整以外，原住民族也有土地再也拿不回來的焦慮。除了因殖民因素被侵佔的公有地、被騙或在不知情狀況簽下的放棄土地文件，樣態實在太多元。政大民族系副教授官大偉就舉例，尖石鄉的前山有一塊地，是日治時期要蓋軍用機場時，政府拿原住民傳統領域跟私人企業換地。他明明是原住民的傳統領域，但按照現在的法令，它就是被排除在外的私有地。</p>
                            <p>而原民會的態度是，這些爭議土地權屬的問題，要放到《土海法》來解決。目前原民會是依照總統蔡英文擔任原住民轉型正義委員會主席在 3 月 20 日做出來的結論，也就是傳統領域劃設辦法分階段實施，初期先排除私有地，並持續推動《原住民族土地與海域法》的立法。</p>
                            <p>《土海法》是根據《原基法》第 20 條，明訂「政府承認原住民族土地及自然資源權利」。但《土海法》由於牽涉太廣，加上是直接規範部落可以有集體使用土地的權利，過去三進三出立法院，都未能成功立法。</p>
                            <p>立委高潞・以用提到，卡關的原因是「各部會、主流社會都還不認識原住民」，原住民族政策協會執行長陳旻園也指出，去年在立法院的時候還被國民黨召委林益世質疑「為什麼拿這種分土地的法案來討論」，讓他感嘆，轉型正義在某些人心中被誤解得十分不堪。</p>
                            <p>那這次原民會哪來的自信，把它當作弭平爭議的關鍵？原民會傳統領域科吳科長指出，關鍵就在去年 8 月 1 日總統蔡英文跟原住民道歉。他提到，總統的政見跟過去有很大的不同，目前正依據政見與道歉文對《土海法》草案做通盤檢討。</p>
                            <p>但原住民族政策協會執行長陳旻園直言，連諮商同意權這麼低的權利，都被限縮在公有地上；土海法涉及的是土地返還、共管等高強度的權利，「怎麼可能會有放寬的機會」。</p>
                            <p>被問及《土海法》立法的時程，原民會表示未來要經過原轉會充分討論後，才進行後續法制作業（最新一次的原轉會因應蘭嶼核廢料調查報告出爐，主題聚焦在真相還原與補償上），雖然被列為優先法案，但實際的時程還無法預估。總統的道歉會是萬靈丹，讓所有的歧視與誤解都消失嗎？</p>`
                },
                {
                  title: `<h1 class="section__issue-title">一條回家的路？</h1>`,
                  subtitle: `<p class="section__subtitle">即使現有的傳統領域劃設辦法爭議被解決，原住民真的就能在傳統領域上做自己想做的事了嗎？原住民回家的路，需要整個社會一起努力⋯⋯</p>`,
                  content: `<p>「原住民好像從出生就是違法的。」阿度蘭阿美斯文化協進會的總幹事蔡政良提到族人在鄰近海域捕魚的狀況，「海巡署先抓再說，部落族人就是先躲再說。原住民就是一直處在這種陰影下，不斷地躲、不斷地逃，這是累積了上百年，在原住民文化底蘊中一個很悲哀的層次⋯⋯。」</p>
                            <p>原住民傳統領域的完整劃設，是為原住民族開了一條回家的路；但原住民要能真正回到自己的家園、做自己想做的事，還有一段路。擋在他們面前的，至少有《礦業法》、《森林法》、《野生動物保護法》、《漁業法》、《槍砲彈藥管制條例》⋯⋯，《原基法》保障原住民權益的精神，碰到其他主管機關的法律，很少打勝仗。</p>
                            <p>雖然相關部會設計了一套「事先報備」的制度，讓原住民能夠因傳統文化、祭儀等非營利目的打獵，但事先報備本身就與打獵文化違背。如布農族的獵人若要上山取獵物，不能大聲張揚，否則就無法獲得山神的贈禮；獵槍還只能用不安全的「土製獵槍」，不合時宜的法律甚至危害到獵人的生命危險。而從 2003 年到現在，已有 382 位原住民因為日常狩獵被判刑，刑期最重是 3 年 6 個月。</p>
                            <p>又如同《礦業法》中地主就算不願意、礦業權人只要提存一筆錢就能逕自開發的相關法條，讓族人就算經過所有權確認的法律長路，拿到了所有權狀，面對土地被礦場霸佔仍莫可奈何。</p>
                            <p>雖然新政府上任後，農委會、林務局陸續發布解釋令，讓原住民在打獵、採取樹木、捕魚都能獲得相關權益保障。《礦業法》也將修正權益不對等的條文，並針對原住民採礦除罪化。去年 6 月因背負 14 顆玫瑰石遭警方依竊盜、違反礦業法起訴的 5 名太魯閣族人，花蓮地方法院也首度援引《原基法》判決在傳統領域採礦無罪。</p>
                            <p>儘管做出對原住民權益更進步的解釋，這個新政府，同時也是讓「採礦權續約得以無視原住民諮商同意權」、「原住民傳統領域劃設辦法排除私有地」的新政府。</p>
                            <p>當總統蔡英文以國家元首的身份道歉，政府內部都出現這樣的歧見，讓人如何相信政府有決心、又要如何取得社會的共識？</p>
                            <p>「加拿大 1982 年建立了新的憲法，政府可以跟原住民訂定新的條約。」政大民族系副教授官大偉說，「它是為了有一個更團結的加拿大。祖先曾經犯錯，但我們知道反省，這是對國家存在的正當性有一個更好的教育。」回首歷史過程，這是每個殖民國家都要面對的不光彩歷史，「不這麼做也可以，只是社會的陰影就會持續存在。」</p>`
                }
              ],
              title: `<h1 class="section__title--invisible section__title" style="justify-self: start">有傳統領域就夠了嗎？</h1>`,
              subtitles: ``
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-6',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">想針對原住民議題有更多的討論，<br>你可以⋯⋯</h1>`,
              subtitles: `<ul class="li--inside">
                            <li class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: .7s"><a style="color: gray" target="_blank" href="https://www.facebook.com/IndigenousTransformativeJusticeTW/?fref=ts">參加原轉小教室，去街頭陪伴還在抗議的原住民</a></li>
                            <li class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: 1.4s"><a style="color: gray" target="_blank" href="https://www.cet-taiwan.org/node/3030">參加環原台灣論壇，瞭解更多原住民族權利</a></li>
                            <li class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: 2.1s"><a style="color: gray">跟原住民青年陣線一起監督政府的原住民政策</a></li>
                          </ul class="li--inside">`
            }
            // {
            //   dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-1',
            //   descriptionModifier: 'left',
            //   theme: 'dark',
            //   hasMore: true,
            //   moreCaption: '以下藏入',
            //   moreTitle: ``,
            //   moreSubtitles: `<p>雖然原民會已有規劃要推動「部落公法人」機制，但目前對這個組織的想像卻還很空泛。</p>
            //                   <p>原住民族政策協進會執行長陳旻園就指出，從傳統領域的劃設、到處理所有權的《土海法》，過去推動這麼多年一直碰到一個問題就是：土地要畫給誰？誰有權利主張？</p>
            //                   <p>「當然，集體意識強的族群比較會講話，但對大部分的族群跟部落來說，內部的討論機制都已經被現在的民意機關、組織架空了。」陳旻園指出，部落裡可能有民族議會、教會、協會等有代表性的組織，但在法律上只有行政機構跟民意代表有法源依據，這是為什麼目前看到的部落代表都是後者。</p>
            //                   <p>陳旻園指出，主體需要定義清楚，部落也需要政府協助培力。他舉劃設辦法為例，要先組一個劃設團隊、再提出範圍計畫、送到鄉公所、核定完之後再送到縣市政府⋯⋯，「程序之複雜，就像過去保留地的增擴編，持續了 20 幾年，還有好幾萬件卡在公部門過不去，這也會造成族人的消極，反正又過不了。」</p>
            //                   <p>「最好的方式，就是主動協助。這不就是主管機關要做的嗎？今天已經有這麼多部落自主宣告，那原民會做了什麼？你要去鼓勵、去幫忙，其他部落看到成效出來，就會覺得我也可以試試看。」陳旻園感嘆，本應該是照顧族人的部會，反而設下這麼高的門檻，像在打小孩給別人看。</p>
            //                   <p>但陳旻園也提醒，原住民也應該準備好，「如果部落沒有主體，還要等待政府的輔佐，等待政府劃一塊傳統領域給你，那跟過去的福利政策有什麼不同？對於社會大眾來說，也會面臨一個問題，原住民憑什麼？永遠就陷在這樣的爭議裡。」</p>`,
            //   title: `<h1 class="section__title--invisible section__title">「誰」可以畫傳統領域？</h1>`,
            //   subtitles: `<p class="section__subtitle">要完整實踐原住民的知情同意權，解決了「在哪裡劃」、「在什麼情形下可以行使權利」的問題以後，還有「誰可以劃」的問題。劃設傳統領域的權利主體是誰？是什麼樣的組織？</p>`
            // }
          ]
        }
      ]
    }
  },
  methods: {
    more () {
      $.fn.fullpage.setAllowScrolling(false)
      $('.hamburger').hasClass('is-active') ? $('.hamburger').removeClass('is-active') : $('.hamburger').addClass('is-active')
      $('.container-more__caption').addClass('container-more__caption--invisible')
      $('.left.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    readIssues (content) {
      $.fn.fullpage.setAllowScrolling(false)
      // $('.hamburger').hasClass('is-active') ? $('.hamburger').removeClass('is-active') : $('.hamburger').addClass('is-active')
      // $('.container-more__caption').addClass('container-more__caption--invisible')
      this.currentSection.currentIssueContent = content
      this.currentSection.showIssueContent = true
      $('.bottom.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    closeIssues () {
      this.currentSection.showIssueContent = false
      $('.bottom.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    moveTo (story, dataStoryIndex) {
      // $.fn.fullpage.setAllowScrolling(false)
      this.currentStory.dataStoryIndex = dataStoryIndex
      // $('.sidebar').sidebar('hide')
      $('#app').animateCss('fadeOut', 'fadeIn', story)
    },
    toggleSidebar () {
      $.fn.fullpage.setAllowScrolling(false)
      $('.right.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    changeCurrentStory (loadedSection, loadedDOM) {
      this.currentStory.dataStoryId = loadedDOM.attr('data-story-id')
      this.currentStory.dataStoryTitle = loadedDOM.attr('data-story-title')
      this.currentStory.dataStoryIndex = Number(loadedDOM.attr('data-story-index'))
      this.currentSection.dataAnchor = loadedSection.dataAnchor
      this.currentSection.hasMore = loadedSection.hasMore
      this.currentSection.moreCaption = loadedSection.moreCaption
      this.currentSection.moreTitle = loadedSection.moreTitle
      this.currentSection.moreSubtitles = loadedSection.moreSubtitles
    },
    storyOnChangeHandler (leavingSection, direction) {
      const leavingSectionHeaderTitleContent = $('.header__title')
      if (this.isStoryChange(leavingSection, direction)) {
        if (leavingSection.attr('data-anchor') === 'landing-page') {
          leavingSectionHeaderTitleContent.animateCss('fadeIn')
        } else if (leavingSection.attr('data-anchor') === 'what-is-going-on-in-KBlvd--section-1' && direction === 'up') {
          leavingSectionHeaderTitleContent.animateCss('fadeOut')
        } else {
          leavingSectionHeaderTitleContent.animateCss('fadeOut', 'fadeIn')
        }
      }
    },
    isStoryChange (leavingSection, direction) {
      const leavingSectionIndex = leavingSection.attr('data-section-index')
      const leavingSectionTotal = leavingSection.attr('data-section-total')
      if (direction === 'up') {
        if (leavingSectionIndex === '1') {
          return true
        } else {
          return false
        }
      } else if (direction === 'down') {
        if (leavingSectionIndex === leavingSectionTotal) {
          return true
        } else {
          return false
        }
      }
    },
    backgroundThemeOnChangeHandler (index, nextIndex) {
      const currentTheme = $($('#fullpage').children()[index - 1]).attr('theme')
      const nextTheme = $($('#fullpage').children()[nextIndex - 1]).attr('theme')
      const classNameList = [
        // 'header__title-container',
        'header__title',
        'partition-nav',
        'partition-nav__content',
        'partition-nav__content--active',
        'partition-nav__content-top-left',
        'partition-nav__content-top-right',
        'partition-nav__content-bottom-left',
        'partition-nav__content-bottom-right'
      ]
      if (currentTheme !== nextTheme) {
        this.changeThemeFromTo(classNameList, currentTheme, nextTheme)
      }
    },
    changeThemeFromTo (classNameList, from, to) {
      for (let className of classNameList) {
        $(`.${className}`).removeClass(`${className}--${from}`)
        $(`.${className}`).addClass(`${className}--${to}`)
      }
    },
    hasChildrenClass (parent, className) {
      return parent.find(`.${className}`).length > 0
    },
    animateVisibleSelector (section, selectors, visible) {
      if (selectors[0] === 'hamburger--background-light' || selectors[0] === 'hamburger--background-dark') {
        if (visible) {
          for (let selector of selectors) {
            $(`.${selector}`).removeClass(`${selector}--invisible`)
          }
        } else {
          for (let selector of selectors) {
            $(`.${selector}`).addClass(`${selector}--invisible`)
          }
        }
      } else {
        if (visible) {
          for (let selector of selectors) {
            section.find(`.${selector}`).removeClass(`${selector}--invisible`)
          }
        } else {
          for (let selector of selectors) {
            section.find(`.${selector}`).addClass(`${selector}--invisible`)
          }
        }
      }
    },
    animateCountUp () {
      var easingFn = function (t, b, c, d) {
        var ts = (t /= d) * t
        var tc = ts * t
        return b + c * (tc + -3 * ts + 3 * t)
      }
      var options = {
        useEasing: true,
        easingFn: easingFn,
        useGrouping: true,
        separator: ',',
        decimal: '.'
      }
      var numAnim = new CountUp('days', 0, this.calculateDays('feb,23,2017,00:00:00'), 0, 3, options)
      numAnim.start()
    },
    calculateDays (countTo) {
      let now = new Date()
      countTo = new Date(countTo)
      let difference = (now - countTo)

      let days = Math.floor(difference / (60 * 60 * 1000 * 24) * 1)
      let years = Math.floor(days / 365)
      if (years > 1) {
        days = days - (years * 365)
      }
      return days
    }
  },
  mounted () {
    const vm = this
    const sections = _.flatMap(vm.stories, (d) => d.sections)

    $(document).ready(() => {
      $('#fullpage').fullpage({
        // menu: '#menu',
        // anchors: ['11', '12', '13', '21', '22', '23'],
        // sectionsColor: ['#C63D0F', '#1BBC9B', '#7E8F7C', '#C63D0F', '#1BBC9B', '#7E8F7C'],
        animateAnchor: false,
        // navigation: true,
        // navigationPosition: 'right',
        afterLoad (anchorLink, index) {
          const loadedSection = $(this)
          vm.animateVisibleSelector(loadedSection, ['section__title', 'section__subtitle-container'], true)
          if (vm.hasChildrenClass(loadedSection, 'self-animate')) {
            vm.animateVisibleSelector(loadedSection, ['self-animate'], true)
          }
          if (vm.hasChildrenClass(loadedSection, 'activity')) {
            vm.animateVisibleSelector(loadedSection, ['activity'], true)
          }
          if (vm.hasChildrenClass(loadedSection, 'x')) {
            vm.animateVisibleSelector(loadedSection, ['x'], true)
          }
          if (vm.hasChildrenClass(loadedSection, 'taiwan-color')) {
            vm.animateVisibleSelector(loadedSection, ['taiwan-color'], true)
          }
          if (sections[index - 1].hasMore) {
            vm.animateVisibleSelector(loadedSection, ['hamburger--background-light', 'container-more__caption'], true)
          }
          if (sections[index - 1].issues) {
            vm.animateVisibleSelector(loadedSection, ['hamburger--background-dark', 'vertical-line', 'space-around__container'], true)
          }
          // vm.animateVisibleSelector(loadedSection, ['verticle-line'], true)
          //
          vm.changeCurrentStory(sections[index - 1], loadedSection)
          if (anchorLink === 'what-is-going-on-in-KBlvd--section-1') {
            vm.animateCountUp()
          }

          if (anchorLink === 'landing-page') {
            $('#bgvid')[0].play()
          }
        },
        onLeave (index, nextIndex, direction) {
          const leavingSection = $(this)
          vm.animateVisibleSelector(leavingSection, ['section__title', 'section__subtitle-container'], false)
          if (vm.hasChildrenClass(leavingSection, 'self-animate')) {
            vm.animateVisibleSelector(leavingSection, ['self-animate'], false)
          }
          if (vm.hasChildrenClass(leavingSection, 'activity')) {
            vm.animateVisibleSelector(leavingSection, ['activity'], false)
          }
          if (vm.hasChildrenClass(leavingSection, 'x')) {
            vm.animateVisibleSelector(leavingSection, ['x'], false)
          }
          if (vm.hasChildrenClass(leavingSection, 'taiwan-color')) {
            vm.animateVisibleSelector(leavingSection, ['taiwan-color'], false)
          }
          if (sections[index - 1].hasMore) {
            vm.animateVisibleSelector(leavingSection, ['hamburger--background-light', 'container-more__caption'], false)
          }
          if (sections[index - 1].issues) {
            vm.animateVisibleSelector(leavingSection, ['hamburger--background-dark', 'vertical-line', 'space-around__container'], false)
          }
          // vm.animateVisibleSelector(leavingSection, ['vertical-line'], false)
          vm.backgroundThemeOnChangeHandler(index, nextIndex)
          vm.storyOnChangeHandler(leavingSection, direction)
        }
      })
    })

    //
    $.fn.extend({
      animateCss: function (animationName, secondaryAnimationName = '', slientMove = '') {
        var animationEnd = 'webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend'
        $(this).removeClass('inactive')
        this.addClass('animated ' + animationName).one(animationEnd, function () {
          $(this).removeClass('animated ' + animationName)
          if (slientMove) {
            $.fn.fullpage.silentMoveTo(slientMove)
            // $.fn.fullpage.setAllowScrolling(true)
            // $('.sidebar').sidebar('hide')
          }
          if (secondaryAnimationName) {
            $(this).addClass('animated ' + secondaryAnimationName).one(animationEnd, function () {
              $(this).removeClass('animated ' + secondaryAnimationName)
              $('.sidebar').sidebar('hide')
            })
          }
        })
      }
    })

    //
    $('#menu')
    .sidebar({
      onHidden () {
        $.fn.fullpage.setAllowScrolling(true)
      },
      context: '#app'
    })
    $('#readmore')
    .sidebar({
      onHidden () {
        // console.log('on hidden')
        $.fn.fullpage.setAllowScrolling(true)
        $('.container-more__caption').removeClass('container-more__caption--invisible')
        this.scrollTop = 0
      },
      onHide () {
        $('.hamburger').removeClass('is-active')
      },
      context: '#app'
    })
    $('#issues-content')
    .sidebar({
      onHidden () {
        // console.log('on hidden')
        $.fn.fullpage.setAllowScrolling(true)
        $('.container-more__caption').removeClass('container-more__caption--invisible')
        this.scrollTop = 0
      },
      onHide () {
        vm.currentSection.showIssueContent = false
      },
      context: '#app'
    })
  }
}
</script>

<style lang="stylus">
@import "../node_modules/fullpage.js/dist/jquery.fullpage.min.css"
@import "../node_modules/animate.css/animate.min.css"
@import "../node_modules/semantic-ui-sidebar/sidebar.min.css"
@import "../node_modules/semantic-ui-menu/menu.min.css"
@import url('//fonts.googleapis.com/earlyaccess/notosanstc.css')
// @import "../node_modules/hamburgers/dist/hamburgers.css"

#app
  animation-duration .5s
  &.pushable
    overflow-x initial

video
  position absolute
  top 50%
  left 50%
  min-width 100%
  min-height 100%
  width auto
  height auto
  z-index -100
  transform translateX(-50%) translateY(-50%)
  // background: url('//demosthenes.info/assets/images/polina.jpg') no-repeat;
  background-size cover
  transition 1s opacity

.sidebar
  width 25% !important
  .item
    height 25% !important
    box-sizing border-box
    border-top 1px solid #142d64 !important
    display flex !important
    justify-content  center !important
    align-items center !important
    font-size 25px
    color #142d64 !important
    font-weight 400 !important
    &.active
      background-color #142d64 !important
      color white !important
.left.sidebar
  width 50% !important
  background-color white
  padding 70px
  font-size 25px
  box-sizing border-box
.ui.bottom.sidebar
  width 90% !important
  height 80% !important
  background-color white
  padding 70px
  font-size 25px
  box-sizing border-box
  margin 5% 5% 5% 5%!important

.container-more
  position absolute
  left 50px
  bottom 30px
  z-index 1000
  display flex
  justify-content center
  align-items center
  &__caption
    color white
    line-height 50px
    margin-left 10px
    opacity 0
    animation-name fadeIn
    animation-duration .7s
    animation-delay .7s
    animation-fill-mode forwards
    &--invisible
      animation-name fadeOut
      animation-duration .1s
      animation-delay 0s
      animation-fill-mode forwards
    // &--fadeIn-when-sidebar-hide
    //   animation-delay .3s

#readmore, #issues-content
  color gray
  h1
    font-size 42px
    font-weight 500
  p
    font-size 20px
    line-height 40px
  .video-container
    margin 5% 0
    position relative
    padding-bottom 56.25%
    padding-top 30px
    height 0 
    overflow hidden
  .video-container iframe, .video-container object, .video-container embed
    position absolute
    top 0
    left 0
    width 100%
    height 100%
  figure
    margin 0
    position relative
    padding-bottom 75.25%
    & + p
      margin-top 5%
    &[class="captionless"]
      padding-bottom 70.25%
    figcaption
      position absolute
      bottom 0
      font-size 18px
    .readmore__img
      position absolute
      width 100%
      height 100%
      background-size contain
      background-position center center
      background-repeat no-repeat

    #dolan1
      background-image url('assets/dolan1.jpg')
    #dolan2
      background-image url('assets/dolan2.jpg')
    #dolan3
      background-image url('assets/dolan3.jpg')
    #dolan4
      background-image url('assets/dolan4.jpg')
    #dolan5
      background-image url('assets/dolan5.jpg')

.section
  // Add for performance issues
  will-change transform
  transform translate3d(0, 0, 0)

  background-size cover
  background-position center center

  // P1
  &[data-anchor="what-is-going-on-in-KBlvd--section-1"]
    background-image url("assets/what-is-going-on-in-KBlvd--section-1.jpg")
  &[data-anchor="what-is-going-on-in-KBlvd--section-2"]
    background-color white
  &[data-anchor="what-is-going-on-in-KBlvd--section-3"]
    background-image url("assets/what-is-going-on-in-KBlvd--section-3.jpg")
    // background-position center 80%
  // P2 
  &[data-anchor="what-is-traditional-indigenous-territories--section-1"]
    background-image url("assets/what-is-traditional-indigenous-territories--section-1.jpg")
  &[data-anchor="what-is-traditional-indigenous-territories--section-2"]
    background-color #142d64
  &[data-anchor="what-is-traditional-indigenous-territories--section-3"]
    background-image url("assets/what-is-traditional-indigenous-territories--section-3.jpg")
  &[data-anchor="what-is-traditional-indigenous-territories--section-4"]
    background-color white
  &[data-anchor="what-is-traditional-indigenous-territories--section-5"]
    background-image url("assets/what-is-traditional-indigenous-territories--section-5.jpg")
  // P3
  &[data-anchor="the-meaning-of-traditional-indigenous-territories--section-1"]
    background-color #142d64 
  &[data-anchor="the-meaning-of-traditional-indigenous-territories--section-2"]
    background-image url("assets/the-meaning-of-traditional-indigenous-territories--section-2.jpg")
  &[data-anchor="the-meaning-of-traditional-indigenous-territories--section-3"]
    background-image url("assets/the-meaning-of-traditional-indigenous-territories--section-3.jpg")
  &[data-anchor="the-meaning-of-traditional-indigenous-territories--section-4"]
    background-image url("assets/the-meaning-of-traditional-indigenous-territories--section-4.jpg")
  &[data-anchor="the-meaning-of-traditional-indigenous-territories--section-5"]
    background-image url("assets/the-meaning-of-traditional-indigenous-territories--section-5.jpg")
    background-position center 20%
  &[data-anchor="the-meaning-of-traditional-indigenous-territories--section-6"]
    background-color white
  &[data-anchor="the-meaning-of-traditional-indigenous-territories--section-7"]
    background-color white
  // P4
  &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-1"]
    background-image url("assets/the-issues-and-solutions-on-traditional-indigenous-territories--section-1.jpg")
    background-position center 20%
  &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-2"]
    background-image url("assets/the-issues-and-solutions-on-traditional-indigenous-territories--section-2.jpg")
  &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-3"]
    background-image url("assets/the-issues-and-solutions-on-traditional-indigenous-territories--section-3.jpg")
  &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-4"]
    background-image url("assets/the-issues-and-solutions-on-traditional-indigenous-territories--section-4.jpg")
  &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-5"]
    background-color #142d64

  &__description
    width auto
    height 100%
    padding 0px 50px
    display flex
    flex-direction column
    &--left
      justify-content center
      align-items flex-start
      text-align left
      max-width 32%
      -webkit-backface-visibility hidden
      background linear-gradient(to left, rgba(0, 0, 0, 0), rgba(0, 0, 0, 1))
      // background url('./assets/test.svg')
      &-wide
        justify-content center
        align-items flex-start
        text-align left
        max-width 64%
        -webkit-backface-visibility hidden
        background linear-gradient(to left, rgba(0, 0, 0, 0), rgba(0, 0, 0, 1))
    &--center
      justify-content center
      align-items center
      text-align center
      padding 0px 15%
      max-width 100%
      &-wide
        justify-content center
        align-items center
        text-align center
        padding 0
        max-width 100%
    &--space-around
      color white
      justify-content center
      // align-items center
      .space-around
        display flex
        flex-direction row
        justify-content space-between
        align-items center
        width 100%
        &>div
          display inline-block
          width 30%
          height 40vh
          margin 2% 0px
          box-sizing border-box
        &__container
          opacity 0
          animation-name fadeInUp
          animation-duration .7s
          animation-fill-mode forwards
          &--invisible
            animation-name fadeOut
            animation-duration .1s
            animation-delay 0s
            animation-fill-mode forwards
      .vertical-line
        width 10px !important
        border-left 1px solid white
        opacity 0
        animation-name fadeInUp
        animation-duration .7s
        animation-fill-mode forwards
        &--invisible
          animation-name fadeOut
          animation-duration .1s
          animation-delay 0s
          animation-fill-mode forwards

  .taiwan-line
    height 90%
    position absolute
    bottom 5%
    right 15%
  .taiwan-color
    height 90%
    position absolute
    bottom 5%
    right 15%
    opacity 0
    animation-name fadeInOpacity85
    animation-duration .7s
    animation-delay .7s
    animation-fill-mode forwards
    &--invisible
      animation-name fadeOut
      animation-duration .1s
      animation-delay 0s
      animation-fill-mode forwards
    @keyframes fadeInOpacity85
      0%
        opacity 0
      100%
        opacity .85

    &.inactive
      transition opacity .7s cubic-bezier(0,0,.2,1)
      opacity 0

#app
  font-family: 'Noto Sans TC', 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

@media (max-width: 1440px)
  .left.sidebar
    padding calc(70px/1.3)
  #readmore
    h1
      font-size calc(42px/1.3)
    p
      font-size calc(20px/1.3)
      line-height calc(40px/1.3)
    figure
      figcaption
        font-size calc(18px/1.3)

@media (max-width: 1280px)
  .left.sidebar
    padding calc(70px/1.5)
  .container-more
    bottom 20px
  #readmore
    h1
      font-size calc(42px/1.5)
    p
      font-size calc(20px/1.5)
      line-height calc(40px/1.5)
    figure
      figcaption
        font-size calc(18px/1.5)

// @media (max-width: 1024px)
//   .left.sidebar
//     padding calc(70px/1.7)
//   #readmore
//     h1
//       font-size calc(46px/1.7)
//     p
//       font-size calc(24px/1.7)
//       line-height calc(44px/1.7)
</style>
