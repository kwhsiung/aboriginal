<template>
  <div id="app">
    <appHeader @toggleSidebar="toggleSidebar()" 
            :dataStoryId="currentStory.dataStoryId" 
            :dataStoryIndex="currentStory.dataStoryIndex"
            :dataSectionIndex="currentSection.dataSectionIndex"
            :theme="currentSection.theme">
    </appHeader>

    <StoryTitle :dataAnchor="currentSection.dataAnchor"
                :dataStoryTitle="currentStory.dataStoryTitle"
                :theme="currentSection.theme">
    </StoryTitle>

    <!-- Sidebar containers -->
    <!-- Navigation sidebar and icon with custom dimmed overlay -->
    <div :class="['custom-dimmed', {'custom-dimmed--visible': sidebarHasToggle}, {'custom-dimmed--invisible': !sidebarHasToggle}]" v-show="sidebarHasToggle" @click="closeSidebar()"></div>  
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
    <!-- Aside container for a section with readmore content -->
    <div class="ui left sidebar vertical menu" id="readmore">
      <div v-html="currentSection.moreTitle"></div>
      <div v-html="currentSection.moreSubtitles"></div>
    </div>
    <!-- Aside container especially for issue topics section with detailed content -->
    <div class="ui bottom sidebar vertical" id="issues-content">
      <div v-html="currentSection.currentIssueContent"></div>
    </div>

    <!-- Buttons -->
    <!-- Absolute position buttons for closing the aside container, could we merge two of this possibly ? -->
    <ButtonMore :theme="'hamburger--background-dark'" 
                :state="'close-issue-btn'"
                :closebtn="'closebtn'"
                @closeissues="closeIssues"
                v-show="currentSection.showIssueContent">
    </ButtonMore>
    <ButtonMore :theme="'hamburger--background-light'" 
                :state="'close-more-btn'"
                :closebtn="'closebtn'"
                @closemore="closeMore"
                v-show="currentSection.showMoreContent">
    </ButtonMore>
    <!-- Absolute position button and caption container for open and reading readmore content -->
    <div class="container-more" v-show="currentSection.hasMore">
      <ButtonMore :state="'open-more-btn'" @more="readMore"></ButtonMore>
      <span class="container-more__caption">{{ currentSection.moreCaption }}</span>
    </div>

    <!-- Main sections, with a pusher class for Semantic UI sidebar module -->
    <div class="pusher">
      <!-- Selector class for Fullpage.js -->
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
                  
            <!-- Dynamically setting leading background video while browsing desktop version, otherwise showing poster image only -->
            <video poster="" id="bgvid" playsinline autoplay muted loop v-if="section.dataAnchor === 'landing-page'">
              <source id="bgvid-source" src="">
            </video>
            <ButtonDown v-if="section.dataAnchor === 'landing-page'"></ButtonDown>

            <!-- Absolute position images for a section explaining with a map -->
            <img class="taiwan-line" src="static/taiwan_line.png" alt="" v-if="section.dataAnchor === 'what-is-traditional-indigenous-territories--section-2'">
            <img class="taiwan-color taiwan-color--invisible" src="static/taiwan_color.png" alt="" v-if="section.dataAnchor === 'what-is-traditional-indigenous-territories--section-2'">

            <div class="photo-credit" v-if="section.hasBackgroundPhoto">{{ section.photoCredit }}</div>

            <Description :class="'section__description section__description--' + section.descriptionModifier">
              <!-- Normal page elements -->
              <div slot="title" v-html="section.title"></div>
              <div slot="subtitle" v-html="section.subtitles" v-if="section.descriptionModifier !== 'space-around' && section.dataAnchor !== 'ending-page'"></div>

              <!-- Issue topics page elements -->
              <div slot="issues" class="space-around" v-if="section.descriptionModifier === 'space-around'">
                <template v-for="(issue, i) in section.issues">
                  <div class="space-around__container space-around__container--invisible" :style="'position: relative; animation-delay: .7s'">
                    <div v-html="issue.title"></div>
                    <div v-html="issue.subtitle"></div>
                    <ButtonMore :state="'open-issue-btn'" :theme="'hamburger--background-dark'" @readissues="readIssues(issue)"></ButtonMore>
                  </div>
                  <div class="vertical-line vertical-line--invisible" :style="'animation-delay: .7s'" v-if="i !== 2"></div>
                </template>
              </div>

              <!-- Ending page elements -->
              <div slot="credits" class="credits" v-html="section.subtitles" v-if="section.dataAnchor === 'ending-page'"></div>
              <RelatedPost slot="related-posts" v-if="section.dataAnchor === 'ending-page'"></RelatedPost>
              <div slot="projects" class="projects__container" v-if="section.dataAnchor === 'ending-page'" onclick="ga('send', 'event', 'projects', 'click', 'projects', { nonInteraction: false })">
                <p class="projects__container-title">看更多專題</p>
                <iframe src="https://mirrormedia.mg/project-list/dark?excluding=20170801aboriginal" width="100%" height="100%" frameborder="0" scrolling="no" style="width: 1px; min-width: 100%; *width: 100%;"></iframe>
              </div>
              <div slot="fbcomments" class="fb-comments__container" v-if="section.dataAnchor === 'ending-page'">
                <div class="fb-comments" data-href="https://www.mirrormedia.mg/projects/20170801aboriginal/" data-colorscheme="dark" data-numposts="1" data-width="100%" data-order-by="reverse_time" style="width: 100%"></div>
              </div>
            </Description>
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
import RelatedPost from './components/RelatedPost'

import $ from 'jquery'
import 'fullpage.js/dist/jquery.fullpage.js'
import CountUp from 'countup.js/dist/countUp.min.js'
import _ from 'lodash'

$.fn.sidebar = require('semantic-ui-sidebar')

export default {
  name: 'app',
  components: {
    appHeader,
    Description,
    ButtonMore,
    ButtonDown,
    StoryTitle,
    RelatedPost
  },
  data () {
    return {
      sidebarHasToggle: false,
      currentStory: {
        dataStoryId: '',
        dataStoryTitle: '',
        dataStoryIndex: undefined
      },
      currentSection: {
        dataSectionIndex: undefined,
        dataAnchor: '',
        hasMore: false,
        moreCaption: '',
        moreTitle: '',
        moreSubtitles: '',
        currentIssueId: '',
        currentIssueContent: ``,
        showIssueContent: false,
        showMoreContent: false,
        theme: ''
      },
      // Recorded object for GA purposes
      haveSeen: {
        1: false,
        2: false,
        3: false,
        4: false,
        5: false,
        6: false,
        7: false,
        8: false,
        9: false,
        10: false,
        11: false,
        12: false,
        13: false,
        14: false,
        15: false,
        16: false,
        17: false,
        18: false,
        19: false,
        20: false,
        21: false,
        22: false,
        23: false
      },
      stories: [
        {
          dataStoryId: 'landing-page',
          dataStoryTitle: '空拍圖',
          sections: [
            {
              dataAnchor: 'landing-page',
              descriptionModifier: 'center',
              theme: 'dark',
              hasMore: false,
              title: `<img src="./static/title.png" id="landing-image" style="position: absolute; left: 50%; right: 50%; transform: translate(-50%, -50%)"></img>`,
              subtitles: ``
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
              hasBackgroundPhoto: true,
              photoCredit: '攝影：楊子磊',
              title: `<h1 class="section__title--invisible section__title">「沒有人是局外人」？</h1>`,
              subtitles: `<p class="section__subtitle">這句最近很紅的口號，從電視新聞、金曲獎頒獎典禮、到在街頭抗議的人們身上都看得見。這句話，起源於 2 月 23 日，原住民歌手巴奈（Panai Kusui）、那布（Nabu Husungan Istanda）和導演馬躍比吼（Mayaw Biho）為了抗議《原住民族土地及部落範圍劃設辦法》，在凱達格蘭大道住了下來，歷經 3 次驅離，目前轉移陣地到台大醫院捷運站前，已經持續了 <span class="section__subtitle section--focus large" id="days"></span> 天。</p>`
            },
            {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-2',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">蔡英文跟原住民族道歉時這麼說</h1>`,
              subtitles: `<p class="section__subtitle section--gray">2016 年 8 月 1 日，總統蔡英文：「臺灣這塊土地，四百年前早有人居住。<br>這些人原本過著自己的生活，有自己的語言、文化、習俗、生活領域。<br>接著，在未經他們同意之下，這塊土地上來了另外一群人。」</p>
                          <p class="section__subtitle section--gray">「歷史的發展是，後來的這一群人，剝奪了原先這一群人的一切。<br>讓他們在最熟悉的土地上流離失所，<br>成為異鄉人，成為非主流，成為邊緣。 」</p>`
            },
            {
              dataAnchor: 'what-is-going-on-in-KBlvd--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              hasBackgroundPhoto: true,
              photoCredit: '照片來源：總統府',
              title: `<h1 class="section__title--invisible section__title">原住民權益<br>原本可以在小英任內大躍進</h1>`,
              subtitles: `<ul class="li--outside">
                            <li class="section__subtitle marginless self-animate self-animate--invisible" style="animation-delay: .7s">首次以總統的高度向原住民道歉</li>
                            <li class="section__subtitle marginless self-animate self-animate--invisible" style="animation-delay: 1.4s">成立了原住民族歷史正義與轉型正義委員會，以總統為召集人</li>
                            <li class="section__subtitle marginless self-animate self-animate--invisible" style="animation-delay: 2.1s">公布《原住民族土地或部落範圍土地劃設辦法》，讓傳統領域終於有了實質效力</li>
                          </ul>
                          <p class="section__subtitle self-animate self-animate--invisible" style="animation-delay: 2.8s; animation-duration: 2s">⋯⋯說是這樣說，但是做法卻出現了爭議！</p>`
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
              hasBackgroundPhoto: true,
              photoCredit: '攝影：楊仁翔',
              title: `<h1 class="section__title--invisible section__title">傳統領域是什麼？</h1>`,
              subtitles: `<p class="section__subtitle marginbottom-less">原住民族土地 ＝ <br>原住民傳統領域 ＋ 原住民保留地（原住民私有地）</p>
                          <p class="section__subtitle">因為「原住民保留地」已經有相關法律處理，所以《原住民族土地或部落範圍土地劃設辦法》處理的是「原住民傳統領域」</p>
                          <p class="section__subtitle section--small">＊之後我們就簡稱《原住民族土地或部落範圍土地劃設辦法》為《劃設辦法》囉！</p>`
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-2',
              descriptionModifier: 'left-wide',
              theme: 'dark',
              hasMore: false,
              hasBackgroundPhoto: true,
              photoCredit: '資料來源：行政院原住民族委員會',
              title: `<p class="section__title--invisible section__title">原住民傳統領域就是原住民生活居住的空間，並不是財產權、私有權概念。<br>原住民的生活跟土地有非常緊密的連結，<br>如打獵、採集、游耕等生活方式都是低密度使用。</p>`,
              subtitles: '<h1 class="section__subtitle">根據原民會的調查，<br>總共有 <span class="section__subtitle section--focus">180</span> 萬公頃，<br>占了整個台灣面積的 <span class="section__subtitle section--focus">50%</span></h1>'
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              hasBackgroundPhoto: true,
              photoCredit: '攝影：林俊耀',
              title: `<h1 class="section__title--invisible section__title">那傳統領域可以幹嘛？</h1>`,
              subtitles: `<p class="section__subtitle">《原基法》第 21 條授權，在傳統領域內一定程度開發行為，應該要向原住民諮商，並取得部落同意或參與，原住民也可以分享相關利益。簡稱「諮商同意權」。</p>`
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-4',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">只有特定行為需要經過原住民族同意</h1>`,
              subtitles: `<br class="remain">
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 1.5s">興建、拓寬、延伸或擴建道路、鐵路、纜車</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 2.0s">商港、漁港、工業專用港、機場、直升機飛行場</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 2.5s">礦石採取</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 3.0s">蓄水、供水、抽水、引水工程及水庫、海水淡化廠、淨水處理廠</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 3.5s">觀光休閒旅館</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 4.0s">發電廠</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 4.5s">有毒物質設施放置</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 5.0s">火化場、公墓、骨灰存放設施</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 5.5s">軍事營區、軍港、海岸巡防營區、飛彈試射場、靶場、雷達站</p>
                          <p class="section__subtitle section--gray marginless activity activity--invisible" style="animation-delay: 6.0s">...</p>
                          <br class="remain">
                          <p class="section__subtitle section--gray section--small marginless activity activity--invisible" style="animation-delay: 7.0s"">＊根據《諮商取得原住民部落同意參與辦法附件》</p>`
            },
            {
              dataAnchor: 'what-is-traditional-indigenous-territories--section-5',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: false,
              hasBackgroundPhoto: true,
              photoCredit: '攝影：林俊耀',
              title: `<h1 class="section__title--invisible section__title">為什麼需要有《劃設辦法》？</h1>`,
              subtitles: `<p class="section__subtitle">就算《原基法》賦予了原住民權利，但過去許多開發案，財團、地方政府都曾主張「法律沒有明確定義原住民傳統領域範圍」，如台東美麗灣開發案、日月潭向山 BOT 案等，都無視原住民的抗議。</p>
                          <p class="section__subtitle">從 2005 年《原基法》立法以來，原住民從來沒有實際實行過諮商同意權。</p>
                          <p class="section__subtitle">所以 2015 年《原基法》修法，明訂主管機關原民會需要針對<span class="section__subtitle section--focus">部落的傳統領域範圍在哪裡、怎麼行使諮商同意權、權益受損如何補償</span>等實務上的細節另訂辦法，就是希望大家都不要再找藉口啦！</p>`
            }
          ]
        },
        {
          dataStoryId: 'the-meaning-of-traditional-indigenous-territories',
          dataStoryTitle: '回不了家的原住民',
          sections: [
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-1',
              descriptionModifier: 'center',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title">為什麼原住民還在街上抗議呢？</h1>`,
              subtitles: `<p class="section__subtitle">原民會在 2017 年 2 月 18 日按照 2015 年修法的結果，<br>
                          公佈了《劃設辦法》，<br>
                          <span class="section__subtitle section--focus">但可以劃的傳統領域卻只剩 80 萬公頃，</span><br>
                          <span class="section__subtitle section--focus">還排除私有地，只能在公有地上劃！</span></p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-2',
              descriptionModifier: 'left',
              theme: 'dark',
              hasBackgroundPhoto: true,
              photoCredit: '攝影：楊仁翔',
              title: `<h1 class="section__title--invisible section__title">消失的 100 萬公頃？</h1>`,
              subtitles: `<p class="section__subtitle"><mark class="mark--blue">Q</mark>：根據原民會過去的調查，傳統領域有 180 萬公頃。現在私有地不能劃，就變成 80 萬公頃嗎？</p>
                          <p class="section__subtitle"><mark class="mark--blue">原民會</mark>：私有地只有 20 幾萬公頃。目前先公告 80 萬公頃，是衡酌部落的能力跟意願。比較沒有能力的部落需要由公家機關協助，根據我們能挹注的經費，估計初步至少能先劃 80 萬公頃。傳統領域真正的範圍是多少，就要看部落，最後當然可能超過 80 萬公頃。</p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              hasBackgroundPhoto: true,
              photoCredit: '鐘聖雄提供',
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
              hasBackgroundPhoto: true,
              photoCredit: '鐘聖雄提供',
              moreTitle: `<h1>讓原住民保有傳統領域沒那麼可怕</h1>`,
              moreSubtitles: `<p>「如果你今天是地主，被原住民劃成傳統領域，你的權益會不會被影響？會不會受到限制？肯定會。」政大民族系副教授官大偉直言，「但限制合不合理，才是要討論的重點。」</p>
                              <p>事實上，在現行的法令中，私有土地本來就會受到各式各樣的限制。都市計畫、區域計畫、土地分區管制，也就是都會區的大樓能蓋多高、農地不能蓋工廠、水源保護區禁止污染事業等等，這些都是地主需要盡的合理社會義務，保障了別人，也保障了自己。</p>
                              <p>官大偉表示，如果限制合理，自然不會有爭議；但假設限制超過地主應盡的社會義務，也有相關補償與救濟的機制。國家擬訂都市計畫也會碰到一樣的問題，補償救濟的條文也很完備，這次卻碰到原住民就轉彎。</p>
                              <p>「論先後順序，原住民傳統領域勢必跟後來出現的國家公有機關、私人產權有所重疊。但難道就無法達到雙贏嗎？」官大偉解釋，傳統領域權是一個很廣泛的概念，權利本來就有很多不同的層次，它可以是「計畫高權」，就像都市計畫規定容積率、建蔽率，或建地、農地等土地使用目的，屬於大方向規劃，規範的土地自然不分公私有；或者也可以是所有權、財產權、管理權、資源使用權、受益權等等，型態很多元。</p>
                              <p>官大偉舉《原基法》第 21 條規定諮商同意權與受益權為例，在傳統領域上的開發行為，需要經過原住民同意，還可以共享利益，直覺讓很多人對傳統領域的肯認打了退堂鼓。</p>
                              <p>但官大偉認為，這不一定是對私人地主的剝奪，「我們在土地上開餐廳、營業，本來就要繳稅給國家。那假設稅收一部分變成原住民的發展基金，透過國家的重新分配，讓轉型正義可以完成，不是很好嗎？」</p>
                              <figure class="long">
                                <img src="./static/villa.jpg"></img>
                                <figcaption>位在台東杉原海岸的美麗灣飯店，除了興建時刻意迴避環評，也因地處原住民傳統領域引發抗議。</figcaption>
                              </figure>
                              <p>而憲法保障的財產權也不是無限上綱，官大偉提到，在私有土地上開發的「限制」方式有很多，原住民族擁有的諮商同意權，是希望鄰近的開發能夠尊重其文化。例如蘭嶼的屋子都是沿著山坡蓋，不管你的房子離海多遠，屋裡的人都能看到海。除了海洋對達悟族人的重要性，還包括觀察海象，看什麼時候可以出海。官大偉說，限制蓋房的高度或區位，這是對空間的管制，不是把你的土地沒收。</p>
                              <p>「這些權利都是傳統領域權的一種，不是全部土地都要收回來才是實踐傳統領域的權利。但這些都需要細細地去談，而不是像現在的做法，碰到困難就轉彎。」官大偉說。</p>`,
              title: `<h1 class="section__title--invisible section__title">關鍵：原住民傳統領域只有諮商同意權一種功能嗎？</h1>`,
              subtitles: `<p class="section__subtitle">原民會土地管理處傳統領域科科長吳秉宸指出，公部門只能依法論法，既然《劃設辦法》是依據《原基法》第 21 條（原住民在傳統領域上有諮商同意權）訂定，那《劃設辦法》劃出來的就只是「諮商同意權範圍」。</p>
                          <p class="section__subtitle">但是，事情真的有這麼簡單嗎？</p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-5',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              moreCaption: '現行劃設辦法會造成立即的侵害！',
              hasBackgroundPhoto: true,
              photoCredit: '鐘聖雄提供',
              moreTitle: `<h1>現行劃設辦法會造成立即的侵害！</h1>`,
              moreSubtitles: `<p>「國土規劃涉及原住民族之土地，應尊重及保存其傳統文化、領域及智慧，並建立互利共榮機制。」這是去年通過的《國土計畫法》第6條第9項，針對國土規劃的基本規則，提及對於原住民權利的保障。</p>
                              <p>《國土計畫法》目前是台灣國土規劃最上位的法律。由中央公告「全國國土計畫」之後，地方政府再依照此原則規劃縣市內的土地使用。過去讓人詬病的浮濫開發、國土保育、甚至原住民權益，都有機會透過這次的盤點重新規劃。國土計畫預計在 2022 年正式上路，相關單位的規劃正如火如荼地進行中。</p>
                              <p>原住民族對土地有很多「移動性」的使用方式，游耕、游牧、狩獵等等，現行法規中卻沒有一個分區可供使用。政大民族系副教授官大偉解釋，台灣的土地使用法規是從大陸的生態系統發展出來，土地很厚，長期從事同一種行為也不致枯竭；但台灣屬於新生島嶼，土壤很薄，在同一塊土地上混用反而更能維護地力。</p>
                              <p>民間倡議了許久，終能在《國土計畫法》中找到邁向自治的契機。《國土計畫法》將重新盤點台灣的土地使用，其中「原住民特定區域計畫」就能讓原住民有用自己文化擬定土地使用的方式，現在內政部區委會也正以新竹鎮西堡部落為先行研究對象，7 月 17 日才剛完成部落現勘。</p>
                              <p>不過，翻出《國土計畫法》的相關條文，關於原住民族的權利範圍都是用「原住民族土地」的字眼。官大偉指出，現在的劃設辦法「定義了傳統領域」，將傳統領域範圍排除了私有地，那麼，未來不管在任何法律中提及「原住民族土地」，傳統領域的定義都有可能直接援引劃設辦法的定義，被限縮在公有地上。</p>
                              <p>也就是說，就算原住民族能從《國土計畫法》中得到依照自己意願使用土地的方式，範圍也只有在公有地和原住民保留地上而已。</p>
                              <span>國土計畫法涉及原住民族權利機制條文：</span>
                              <table class="mobile-hide">
                                <tr>
                                  <td>第 6 條</td>
                                  <td>國土規劃涉及<u>原住民族之土地</u>，應尊重及保存其傳統文化、領域及智慧，並建立互利共榮機制。</td>
                                </tr>
                                <tr>
                                  <td>第 11 條</td>
                                  <td>國土計畫中之特定區域涉及<u>原住民族土地</u>及海域者，其內容需由中央主管機關會同原民會擬定，並依原基法21條規定辦理。</td>
                                </tr>
                                <tr>
                                  <td>第 23 條</td>
                                  <td>國土功能分區及其分類之使用地類別編定、變更、規模、可建築用地及其強度、應經申請同意使用項目、條件、程序、免經申請同意使用項目、禁止或限制使用及其他應遵行之土地使用管制事項之規則，涉及<u>原住民族土地</u>者，其內容需由中央主管機關會同原民會擬定，並依原基法21條規定辦理。</td>
                                </tr>
                                <tr>
                                  <td>第 20 條</td>
                                  <td>國土分區中之海洋資源區的劃設需考量原住民族傳統使用。</td>
                                </tr>
                                <tr>
                                  <td>第 36 條</td>
                                  <td>國土復育促進地區經劃定者，應以保育和禁止開發行為及設施之設置為原則，並由劃定機關擬訂復育計畫，報請中央目的事業主管機關核定後實施。如涉及<u>原住民族土地</u>，劃定機關應邀請原住民族部落參與計畫之擬定、執行與管理。</td>
                                </tr>
                              </table>
                              <div class="desktop-hide">
                                <p style="margin: 5px 0px"><span style="font-weight: 500;">第 6 條：</span><br>國土規劃涉及<u>原住民族之土地</u>，應尊重及保存其傳統文化、領域及智慧，並建立互利共榮機制。</p>
                                <div style="width: 100%; height: 2px; background: gray"></div>
                                <p style="margin: 5px 0px"><span style="font-weight: 500;">第 11 條：</span><br>國土計畫中之特定區域涉及<u>原住民族土地</u>及海域者，其內容需由中央主管機關會同原民會擬定，並依原基法21條規定辦理。</p>
                                <div style="width: 100%; height: 2px; background: gray"></div>
                                <p style="margin: 5px 0px"><span style="font-weight: 500;">第 23 條：</span><br>國土功能分區及其分類之使用地類別編定、變更、規模、可建築用地及其強度、應經申請同意使用項目、條件、程序、免經申請同意使用項目、禁止或限制使用及其他應遵行之土地使用管制事項之規則，涉及<u>原住民族土地</u>者，其內容需由中央主管機關會同原民會擬定，並依原基法21條規定辦理。</p>
                                <div style="width: 100%; height: 2px; background: gray"></div>
                                <p style="margin: 5px 0px"><span style="font-weight: 500;">第 20 條：</span><br>國土分區中之海洋資源區的劃設需考量原住民族傳統使用。</p>
                                <div style="width: 100%; height: 2px; background: gray"></div>
                                <p style="margin: 5px 0px"><span style="font-weight: 500;">第 36 條：</span><br>國土復育促進地區經劃定者，應以保育和禁止開發行為及設施之設置為原則，並由劃定機關擬訂復育計畫，報請中央目的事業主管機關核定後實施。如涉及<u>原住民族土地</u>，劃定機關應邀請原住民族部落參與計畫之擬定、執行與管理。</p>
                              </div>
                              <span>（資料來源：官大偉研究室）</span>`,
              title: `<h1 class="section__title--invisible section__title" style="margin-top: 10px;">劃設辦法只是起步，</h1>
                      <h1 class="section__title--invisible section__title">不應該成為傳統領域的定義！</h1>`,
              subtitles: `<p class="section__subtitle">按照總統蔡英文 3 月 20 日在原轉會的結論，若讓原住民在私有地行使同意權，有侵害私人財產權的疑慮，應該要分階段處理，先讓傳統領域排除私有地，再推動《土海法》，解決歷史上以不正義手段取得的原住民土地。</p>
                          <p class="section__subtitle">但，這等於是將傳統領域的範圍限縮在公有地上！</p>
                          <p class="section__subtitle">依照現行的法律，原住民在傳統領域上只能行使諮商同意權，但不代表未來都是如此。<br>例如去年才通過的、預計 2022 年上路的《國土計畫法》，原住民就能在傳統領域上依照自己的意願規劃使用土地。</p>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-6',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">政府的做法為什麼引起爭議</h1>`,
              subtitles: `<div class="li--inside">
                            <span class="x x--invisible" id="x1"></span>
                            <p class="section__subtitle section--gray self-animate self-animate--invisible" style="animation-delay: .7s">由於有侵害個人財產權的疑慮，劃設辦法分階段實施，現在先處理公有土地</p>
                            <p class="section__subtitle section--red self-animate self-animate--invisible" style="animation-delay: 2.1s; margin: 0">可是，這樣就將原住民傳統領域限縮在公有土地上了</p>
                          </div>
                          <div class="li--inside" style="margin-top: 5%">
                            <span class="x x--invisible" id="x2"></span>
                            <p class="section__subtitle section--gray self-animate self-animate--invisible" style="animation-delay: 2.8s">私有土地的部分未來將推動《原住民族土地及海域法》處理</p>
                            <p class="section__subtitle section--red self-animate self-animate--invisible" style="animation-delay: 4.2s; margin: 0">可是，連低強度的知情同意權都引起這麼大的爭議，<br>土海法處理的是所有權返還、資源共管這種更進一步的權利，<br>非常困難</p>
                          </div>`
            },
            {
              dataAnchor: 'the-meaning-of-traditional-indigenous-territories--section-7',
              descriptionModifier: 'center',
              theme: 'light',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title section--gray">民間建議這樣做</h1>`,
              subtitles: `<div class="li--inside mobile-marginless">
                            <p class="section__subtitle section--gray self-animate self-animate--invisible" style="animation-delay: .7s">退回現行的傳統領域劃設辦法</p>
                            <p class="section__subtitle section--gray self-animate self-animate--invisible" style="animation-delay: 1.4s">既然目前只擔心侵害個人財產權，<br>那就將「哪些行為需經過原住民諮商同意」再定義清楚，<br>修改《諮商取得原住民部落同意參與辦法附件》就好了</p>
                            <p class="section__subtitle section--gray self-animate self-animate--invisible" style="animation-delay: 2.1s">將劃設辦法中的文字更明確地修改為「劃設公有地中的原住民族傳統領域」，<br>而非「原住民族傳統領域排除私有地」</p>
                            <p class="section__subtitle section--small list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: 2.8s">＊諮詢對象：政治大學民族系副教授官大偉、臺北醫學大學醫學人文研究所所長林益仁</p>
                          </div>`
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
              hasBackgroundPhoto: true,
              photoCredit: '攝影：林俊耀',
              moreTitle: `<h1>台東都蘭部落：傳統領域就是家</h1>`,
              moreSubtitles: `<p>一個下著大雨的夜晚，我們在都蘭海邊盯著兩個海上的光點。一個多小時後，先上岸的是比較資深的一南（Yinam），手裡的籃子有海膽、海螺跟一條魚。他用的是漁槍，因為這幾天下雨，水不夠清澈，造成視線不佳，並不是捕魚的好時機。</p>
                              <p>但在等待一南的過程中，也有兩個阿美族人上岸來。兩個人用的是漁網，捕到約莫六、七隻魚，我們一邊驚嘆，他們只淡淡地說「一天就吃完啦」。</p>
                              <video class="morevid" poster="./static/dulan-morevid-poster.png" controls style="position: static; width: 100%; height: 100%;">
                                <source src="./static/dulan.mp4">
                              </video>
                              <p>台東縣都蘭部落在 2 月 28 日自主宣告傳統領域，總共 74 個地名，連海洋也算在內。部落靠海，親近海洋被認為是理所當然的活動，如果年輕人很久沒去、或明明工作放假還待在家裡不下海，會被長輩視為懶惰。</p>
                              <p>第一次「捕魚」，是國小三年級中午放學下課跟表弟去海邊幫爸爸收網。一南記得當天的浪很大，「我帶著游泳圈就下去了，邊哭邊收耶！風很大很害怕，水母又多，以前又沒有潛水衣，穿著內褲就下去了。上岸的時候覺得命還在算是撿到的吧！」</p>
                              <p>真正潛水打魚，是一南當兵後的事了。他越級三層，來到哥哥們的年齡階層，跟著哥哥一起下水打魚，學習使用魚槍、看海況等各種捕魚的知識。三年之後，他回到自己的年齡階層，將知識與經驗分享給同儕。</p>
                              <figure>
                                <img src="./static/dulan1.jpg"></img>
                                <figcaption>一南（左）與年齡階層較年輕的阿倫（右）一同入海捕魚。</figcaption>
                              </figure>
                              <figure>
                                <img src="./static/dulan2.jpg"></img>
                                <figcaption>兩人在上岸後分享捕到的漁獲。由於海水能見度不佳，多是撿拾的貝類。</figcaption>
                              </figure>
                              <p>在一旁的台東大學助理教授蔡政良遞上啤酒。蔡政良原為客家人，後來擬親成為阿美族，現在是阿度蘭阿美斯文化協進會的總幹事。他與一南屬於同一個年齡階級，目前是部落的「策動組」，是組織活動運轉的核心。</p>
                              <p>蔡政良形容，在得知要接下策動組重責大任的前幾年，年齡階層中的同儕都很緊張。但不約而同，大家都會約去海邊打魚，像個心照不宣的默契，「在海邊打魚的時候，生命是交給彼此的。我們曾經出事過，也把彼此救回來。」</p>
                              <p>蔡政良說：「過去沒有承擔責任、權力的時候，不會感受到。但一起去海邊打魚，就讓我們階層團結起來。」海洋陪著部落男孩長大，從部落的文化傳承、生活所需，到成為一個男人需負擔的責任，它全讓族人來懷裡索取。</p>
                              <p>正是因為海洋與生活的密切結合，都蘭部落對於欲私有化海灘的開發案抗爭不遺餘力，如知名的反美麗灣運動。儘管成功擋下，比美麗灣大數倍的杉原灣度假村，仍在今年初通過了環評委員的審查，顯示財團對東海岸的野心從未停息。</p>
                              <p>都蘭部落自主宣告的傳統領域不分公私有，但「權利」卻公私有別。如「須諮詢部落取得同意」後才能做的事，是不分公私有土地內的「經濟性開發行為」及「牽涉倫理的人為活動」；而涉及部落「共享利益」，卻僅限於「公產機關在傳統領域上的經濟利益」。</p>
                              <p>蔡政良提及，這就是傳統領域跟私人財產權無關的證明，「諮商同意權就是你來我家附近當鄰居，當然要打聲招呼，部落的人都相當清楚，要有一定規模的開發，如超過一公頃、或是蓋下去會引發土石流、影響飲用水等等，才要諮詢。否則三天兩頭就召開部落會議，哪有那麼多時間？」</p>
                              <p>但現行排除私有地的傳統領域劃設辦法，讓都蘭部落面臨了立即的威脅。去年開始，欲在知名景點「水往上流」附近興建 500 個房間規模的 ATT 度假村，就全部是私有地。雖然興建度假村仍須經過環評，但部落族人對於一個在「家」附近的開發案居然毫無置喙空間。</p>
                              <p>「這幾年面對東海岸的開發案，部落老人家有一種很深的焦慮，面對土地慢慢流失，拿不回來。但我們也很清楚，按照現在的劃設辦法，土地的主權不會有改變。只有諮商同意權、利益共享而已。」蔡政良說，「但在心理層面上，有了諮商同意權，是認定原住民是土地的主人。還是比較正向的發展。」</p>
                              <p>訪問過程突然雷聲大作，一南不自覺地抖了一下，斜眼看著天空苦笑，說「趕快跑呦」，要是知道會打雷，絕對不下海。他們收拾東西，在愈下愈大的雨裡準備去蔡政良家喝魚湯了。我趕緊追問一南，海是什麼呢？</p>
                              <p>「海啊，就是不管我再怎麼累，一看到海，整個心情就好輕鬆，下去游一游，就不累了。在家裡看電視還會打瞌睡，還會被老婆唸啊，但一跳進海裡，什麼煩惱都沒有了。跟朋友們也是有空就約在海邊聯絡感情。」一南說，「大海不只幫助阿美族的生活，包括感情聯繫、儀式祭典都在海邊，讓大家可以聚在一起。」</p>
                              <p>在阿美族的族語裡，並沒有「傳統領域」這個字。對他們來說，傳統領域就是「家」。</p>
                              <figure>
                                <img src="./static/dulan3.jpg"></img>
                                <figcaption>打完魚後，一南會跟同年齡階級的蔡政良一起煮魚湯、聊天。</figcaption>
                              </figure>
                              <figure>
                                <img src="./static/dulan4.jpg"></img>
                                <figcaption>一南說，不管再怎麼累，看到海心情就會非常輕鬆。</figcaption>
                              </figure>
                              <figure>
                                <img src="./static/dulan5.jpg"></img>
                                <figcaption>用魚槍打魚重視水的清澈度。</figcaption>
                              </figure>`,
              title: `<h1 class="section__title--invisible section__title">台東都蘭部落：<br><span class="section--focus">傳統領域就是家<span></h1>`,
              subtitles: `<p class="section__subtitle">倚海而居的台東縣阿美族都蘭部落，在今年 2 月自主宣告了傳統領域，連海洋也包含在內。傳統領域不分公私有地，但在部落的管制規則中，權利卻公私有別，僅公有機關的開發案需共享利益，私有地不用；但私有地具規模的開發仍須經部落同意，他們認為這是一種「當鄰居前的告知」。東海岸面臨的開發威脅從未少過，現在又有完全是私有地的度假村要來闖關⋯⋯</p>`
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-2',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              hasBackgroundPhoto: true,
              photoCredit: '攝影：林俊耀',
              moreCaption: '看台東卡大地布部落的故事',
              moreTitle: `<h1>台東卡大地布部落：傳統領域就是主權</h1>`,
              moreSubtitles: `<p>「像現在下雨，山豬搞不好凌晨的時候就走過一遍了。」卑南族獵人曾文德（Asiyan）在大雨中走入比人高的雜草，頓時讓我們慌了手腳，接著才發現腳下有路。他邊整理之前放的陷阱，邊跟我們解釋獵山豬的原因。</p>
                              <p>「為什麼要在這裡打獵，除了守護傳統領域，這是我們的獵區；另外就是現在山豬已經很少人捕獵，只有在原住民的歲時祭儀才會去山上捕。」曾文德提到，「我家的地就在下面，種的花生、玉米、甚至於竹子，牠們都去挖起來吃。」</p>
                              <video class="morevid" poster="./static/katratripulr-morevid-poster.png" controls style="position: static; width: 100%; height: 100%;">
                                <source src="./static/katratripulr.mp4">
                              </video>
                              <p>「現在打獵，也都是以獵槍為主，雖然命中率比較高，但我們原住民還是比較喜歡用傳統的方式。」曾文德形容，以放陷阱的方式打獵，就像在跟動物鬥智。要先觀察牠喜歡吃的東西、喜歡在哪裡駐足、曾經出現在哪裡，鎖定了地點以後，接下來就只能等待。</p>
                              <p>「其實山豬是很聰明的，牠的鼻子很靈。像我們剛剛放陷阱，就不可能馬上獵到，因為牠會聞得到人味、體味，只要是他不對勁的味道他就不會來。」曾文德解釋，除了等待之外，另一種方式就是讓山豬習慣獵人的味道，牠可能就覺得你只是例行性巡山，沒有威脅性。習慣了以後，山豬就會放下戒心。</p>
                              <p>不到 20 分鐘，他已經修完陷阱準備離開，就像巡視家裡後院一般平凡。曾文德說，平時上山目的很多，有人是想抒發心情，想到就會上山看看，「這裡是我們祖先的地方，很久沒有人上來就煙草瀰漫，會對不起祖先，（祖先會覺得）都沒有人來看我們。」</p>
                              <figure>
                                <img src="./static/katratripulr1.jpg"></img>
                                <figcaption>曾文德說，雖然用獵槍打獵很方便，他們還是比較喜歡用傳統的方式。</figcaption>
                              </figure>
                              <p>曾文德提到，這一片山，現在由林務局掌管，但過去是卡大地布瑪琺琉（Mavaliw）家族所在，統治整個東台灣，「山下看到的所有部落，都是要向他進貢的。所以這裡是我們的傳統領域，有必要要去巡守。現在我們不住在這了，但獵物還是跑在這裡。（我們就）一方面巡守著山林，一方面延續著我們的獵人文化。」</p>
                              <p>卡大地布部落的傳統領域，東起利嘉溪口至華源村南坑海岸、西至東屏交界的霧頭山知本溪源頭、南至華源村羅打結山至知本溪源頭、北到追分山至巴油池稜線。以 3 個祖先當初從現在的蘭嶼、綠島中間登陸升起的台灣島為起點，各自遷徙、發展，到 300 年前才形成卡大地布部落。</p>
                              <p>每年在小米收穫祭後，部落的男人就會去獵場巡視，因為土地滋養了部落的族人，族人就必須守護土地，也因此，守護家園是卑南族男人一生最大的使命。</p>
                              <p>獵場巡視過程中如果遇到外面的人，經勸阻、警告後若仍未離開，就會出草。巡視完獵場後，他們會把人頭掛在部落前，有威嚇的意味，再上山去取獵物回來餵人頭，卡大地布族人高明智解釋，「意思是謝謝你被我砍頭，讓我的領域完整了。」</p>
                              <p>「所以真正的打獵，是砍人頭的意思。」高明智看來有點難為情，「我們平常不會說什麼打獵啦，早上帶你們去放陷阱，只是日常生活啦！」</p>
                              <figure>
                                <img src="./static/katratripulr4.jpg"></img>
                                <figcaption>曾文德提到，因應不同的獵物要用不同的獵具，也要猜測獵物可能的行蹤，就像在鬥智。</figcaption>
                              </figure>
                              <p>2007年，卡大地布部落在歷經十幾年的文化復振後，重新執行古老的祭典。卻在進行大獵祭時，被森林警察追捕，造成儀式中斷，引發衝突。可以想見，這對族人來說是嚴重的污辱。</p>
                              <p>高明智說，因為那次的衝突，族人才突然發現中華民國有《原住民基本法》，原住民享有一些權利。他很感嘆，身為一個在戒嚴時期長大的小孩，槍桿子底下的高壓政權讓他遠離部落，連回家的路都走得艱辛。</p>
                              <p>還好從 1990 年開始的文化復振，漸漸團結了族人。從 2007 年的大獵祭、到 2010 年的反遷葬，族人為了保護傳統領域，不惜與公部門槓上，還吃上侮辱公署、妨害公務等官司，後來選上市民代表的陳政宗就是其中之一。</p>
                              <p>陳政宗是兄長輩進行文化復振時，回到部落的孩子。從日治到國民政府殖民，都對部落文化造成深遠的影響，如戒嚴時期規定不准集會，造成巴拉冠（卑南成年男子集會所）的瓦解、又禁止說母語⋯⋯，「我不會講自己的話，不知道自己屬於哪個階級，求學的時候，人家問你是誰，我說原住民，對方就會冒出很多刻板的印象，也連帶我自己反思我到底是誰。」</p>
                              <p>有了疑問，他決定回到部落。所謂的文化傳承實踐在生活中，投入祭典籌備、聽老人家說故事，「其實傳統領域的概念也是近年才出現。對我們來說，土地就是生存，你曾經生活在哪塊土地上，就要瞭解土地帶給你的意義是什麼。那時候才知道，原來土地不只是文字上的意義，而是生存的智慧。」陳政宗說。</p>
                              <figure>
                                <img src="./static/katratripulr2.jpg"></img>
                                <figcaption>高明智腳下是部落祖先的墓地，縣政府曾因觀光需求要直接遷墓，引發族人激烈抗爭。</figcaption>
                              </figure>
                              <p>陳政宗提到，「再繼續探究，為什麼大獵祭的時候，回到獵場狩獵，會有這麼多限制？或是小米收穫祭，只是種植平日生活所需，會有這麼多關卡？甚至回到自己的土地生活，還會被抓，那，土地到底是怎麼流失的？」</p>
                              <p>「我們卡大地布在這裡已經幾百年，以狩獵、游耕的方式安穩生活，卻在這短短的一百年內，被壓縮成現代化的樣子。我們想用自己的方式生活，卻都是犯法的。我們是被你們的槍壓在底下，不得不服。」在一旁的高明智補充。</p>
                              <p>歷經族人的戮力爭取，縣政府終於讓步，原本為了觀光的遷葬案後來變成卡地布追思文化園區，看似是一場風光的勝利。但即便總統都道歉了、原民會公布傳統領域劃設辦法、卡大地步也在今年 3 月自主宣告傳統領域，還是發生了這樣的事情⋯⋯</p>
                              <p>「我不知道那是你們的傳統領域。」2017 年 7 月 4 日，台東縣政府的一場說明會上，火藥味十足。說話的是台東縣建設處長吳慶榮，縣政府欲進行知本溪疏濬工程，不但未先取得卡大地布部落同意，還欲將砂石堆置在知本溪流域。吳慶榮表示，縣府沒有預算配合部落的要求，將砂石遷移到別的地方，部落要做可以自己募款。</p>
                              <p>這片要被堆積沙土的知本溼地，已經不是第一次遭到踐踏。</p>
                              <p>知本溼地被卡大地布部落稱為 Tavurung，意即水流匯集的地方。傳說荷蘭人因為看到黃金，從知本溪口登陸，遇上以會發出巨響的木棒擊斃水鹿的卡大地布族人，將此地命名為 Kinkuwangan，是槍響的意思。</p>
                              <p>但 1982 年，知本溼地被納入綜合遊樂區開發案，以 50 年租約的方式交給傑帝爾開發公司開闢高爾夫球場，高明智帶我們到一片荒煙蔓草的空地，「傑帝爾開發失敗，就荒廢在這裡，長達 30 年了，我們還是要不回自己的土地。」他憶起當年，族人跟縣政府與開發公司抗議這裡有潟湖，「他們就用怪手把潟湖推掉，真的給族人上了一課。告訴我們，想要開發，就能夠睜眼說瞎話。」</p>
                              <p>新政府上台後，台東縣政府又宣稱將在這裡蓋「光電示範區」推廣再生能源。30多年來，對於傳統領域要做什麼，族人從來沒能插得上話。</p>
                              <p>於是，卡大地布部落才決定在 2017年 2 月自主宣告的傳統領域，「傳統領域對我們來說，就是主權。我們要生在這裡、長在這裡、死在這裡。」高明智說。</p>
                              <figure class="long">
                                <img src="./static/katratripulr3.jpg"></img>
                                <figcaption>卡大地布部落的歷史，是 3 個祖先當初從現在的蘭嶼、綠島中間登陸升起的台灣島為起點，各自遷徙、發展，到 300 年前才形成卡大地布。</figcaption>
                              </figure>
                              <p>採訪尾聲，聚集在巴拉冠旁邊的族人聊到一個即將成形的計畫，他們在荒廢已久的傑帝爾園區附近找到一塊地，準備去農耕開墾，宣示土地屬於部落。</p>
                              <p>「要讓他們知道我們要守護土地的決心，不是只有說說而已！」「說得好！」「我那裡有種子耶！」「我也有啊！」部落的男子們雀躍著呼應與計畫著，團結的心就這樣自然匯集成行動的力量，繼續向前。</p>
                              <p>卡大地布的意思是「在一起，不分開」。而對族人來說，保護傳統領域的意識，即使過了幾百年，仍像戰鬥一樣必須戮力，而他們從不退縮。</p>`,
              title: `<h1 class="section__title--invisible section__title">台東卡大地布部落：<br><span class="section--focus">傳統領域就是主權</span></h1>`,
              subtitles: `<p class="section__subtitle">每年小米收穫祭後，卑南族的男人就會去獵場巡視，因為土地滋養了部落的族人，族人就必須守護土地，也因此，守護家園是部落男人一生最大的使命。卡大地布透過幾十年的文化復振找回部落的文化與團結，但傳統領域面臨的開發危機，還是不斷襲來⋯⋯</p>`// TODOs: TBA
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-3',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              hasBackgroundPhoto: true,
              photoCredit: '攝影：林俊耀',
              moreCaption: '看來吉不舞的故事',
              moreTitle: `<h1>阿里山來吉部落：人與自然的戀曲</h1>`,
              moreSubtitles: `<p>來到阿里山來吉部落，沿路可見一個個鮮豔的山豬石雕，成為部落鮮明的標誌，還有人稱這裡是「山豬公園」、「山豬部落」。相傳當年鄒族獵人為了追逐山豬，來到了一塊肥沃的土地，決定在此定居，但這些山豬石雕可不單單只是因為這個故事。</p>
                              <figure>
                                <img src="./static/pnguu1.jpg"></img>
                                <figcaption>沿路可見的美麗山豬石雕現在是來吉部落的標誌。</figcaption>
                              </figure>
                              <p>這些美麗的石雕藝術，最早出自藝術家不舞的手，山豬幾乎成為她創作的招牌。不舞並不是在部落長大的孩子，當年只是因為在部落經營小吃店的母親希望她回來幫忙，她就此留了下來。</p>
                              <p>「其實媽媽蠻特別的，一般的父母不是都希望孩子能在外面好好發展？好好工作、賺錢，但我媽就會叫我回山上幫她的忙。」不舞提到，她從小就對畫畫有興趣，回到部落，或許也能做自己想做的事，「我看山上的人都胖胖的啊！雖然可能賺不到什麼錢，但好像也過得不錯。」她笑說。</p>
                              <video class="morevid" poster="./static/pnguu-morevid-poster.png" controls style="position: static; width: 100%; height: 100%;">
                                <source src="./static/pnguu.mp4">
                              </video>
                              <p>回到部落以後，她從跟老人家的閒聊中開始瞭解鄒族的文化故事，對此深深著迷，也變成了她創作的養分。</p>
                              <p>如鄒族的聖山塔山，「我覺得塔山給我的感覺非常美好、溫暖，看到這座山就好像有依靠」或是孤挺花，「孤挺花是族人喜歡配戴的頭花，我外婆說，部落有很多遺跡，是過去的人留下的生活證據，但現在都消失了。不過你可以找到孤挺花，是以前的族人種的，它很美，而且不必太刻意照顧也能開出美麗的花朵」，不舞信手拈來都是故事，也象徵著族人的生活方式。</p>
                              <p>那山豬呢？不舞曾經有一頭寵物山豬，是媽媽送她的。她還曾經跟山豬約定，要牠變成全世界最老的山豬，但有次她回家，卻發現山豬被媽媽殺掉了。後來不舞夢到了牠，從此開始做跟山豬有關的創作，並從中得到療癒。</p>
                              <p>「一開始做山豬石雕，族人會有很多問號，覺得這能代表部落嗎？但當外面的遊客看到這樣的作品，好像蠻開心的，也會跟它們合照，分享出去。」不舞提到，後來這也變成八八風災後部落重新站起來的力量，利用「山豬部落」來對外行銷，她也把工藝技術帶進來，現在在部落裡的石雕，有些出自其他族人的手。</p>
                              <figure>
                                <img src="./static/pnguu2.jpg"></img>
                                <figcaption>不舞以山豬當作品牌，從藝術品到有機茶都以山豬為主題。</figcaption>
                              </figure>
                              <p>不舞不只是藝術家，還是原住民中第一個拿到有機茶認證的農夫。大約 15 年前，不舞說當時也不是很瞭解「有機」的概念，只覺得有機對人是友善的，按照原住民的生活方式，就是要走有機。</p>
                              <p>我們來到不舞的茶園，茶樹長得很高，還高矮不一，錯落著雜草，幾乎把人都淹沒。「做有機很孤獨，也很沒有效率，但最起碼，我們可以喝到健康無毒的茶。」不舞說。走了十幾年，在外人看來相當辛苦的事，她們僅靠著簡單的信念就走過來。</p>
                              <p>「其實從外人看來，原住民的生活方式很沒有經濟效益啊。但過去幾百年，我們就是這樣生活的，不想著賺錢，就能自給自足。」不舞提到家族的歷史，阿古雅那家族的傳統領域就在現在的阿里山遊樂區，甚至「阿里山」之所以這樣命名，就是來自家族內的厲害首領阿巴里。</p>
                              <p>不舞提到，以前鄒族人管理傳統領域，大家都維護得很好，因為這是屬於你的獵場，不能讓資源枯竭。不管是河流裡的魚、山裡的獵物、哪些土地可以種田，如果數量開始減少、地力有枯竭的跡象，就像換另外一區，大家會跑來跑去。很多行為背後都有智慧，就像原住民會砍掉一些樹木做小範圍的耕種，是為了讓光線透進來，鳥類會變多，生物的多樣性就會增加。</p>
                              <p>「以前的林相，大樹都不能砍，所以才有破千年的檜木林。因為萬物都有靈，你也可以跟森林對話，不過以前的人走到森林裡是不講話的，因為會吵到神靈。在森林裡唯一的聲音就是唱歌，或是敲擊樂器，用這樣的方式來跟神靈說話。」不舞說，「以前的人面對自然環境，是彼此尊重」。</p>
                              <figure>
                                <img src="./static/pnguu3.jpg"></img>
                                <figcaption>不舞的家門口正對著鄒族的聖山，塔山。</figcaption>
                              </figure>
                              <p>但他提到，土地孕育了鄒族人，在這四百年遇到荷蘭人、日本人、到現在的國民政府，雖然四百年相比於千年的歷史很短，但在這麼短的時間內，島嶼內的很多事情卻被消耗了，文化、語言都快要消失。</p>
                              <p>「像現在的阿里山神木，它們過去跟原住民生活了很久很久，現在很多卻只剩下檜木頭。過去的美好林相，現在都變成遺跡了。」不舞提到，甚至後來有一陣子台灣得靠賣樹維生，在族人看來是非常傷心的事。</p>
                              <p>「阿里山有個姊妹潭，我外公的爸爸他的工寮就在那裡。日治時代的時候，日本人有畫界碑，這個區域妳要在這裡做任何工作，你要跟原住民租。」不舞說，後來國民政府來台，外公不想租了，就收回來做了一個歌舞團。後來發生了一場火災，國家做了暫時的安置，隔一兩年，大家又回到自己的部落。</p>
                              <p>但政府後來為了興建阿里山森林遊樂區，「當時國民政府說你有在使用的，跟國家登記，剩下全部是國家的。」不舞無奈地說，「但以前的人沒有說這是自己的，我們都會說那是家族的。後來阿里山森林遊樂區都是林務局在管，現在是看不到原住民的。」</p>
                              <figure>
                                <img src="./static/pnguu4.jpg"></img>
                                <figcaption>阿里山森林遊樂區也是鄒族的傳統領域範圍。</figcaption>
                              </figure>
                              <p>她提到，國家就蓋了一座門來收門票。「我們有時候走到（遊樂區），不管是打獵、還是⋯⋯，也都會覺得，到底（我們）是主人還是客人？到那邊我們還是要報備啊，我們是哪一村的，因為我們是原住民才不收門票。」不舞苦笑，述說著矛盾的心情。</p>
                              <p>在原住民傳統領域劃設辦法公布之後，雖然爭議四起，原民會卻也同時強調目前已有 139 個部落、 3 個民族申請劃設，顯見這是原住民族長期共同期待的目標。阿里山鄒族也在也在「搶先申請」的名單之列。</p>
                              <p>在鄉公所參與過傳統領域調查專案的鄒族族人湯文賢表示，調查起源於原民會的先期示範計畫，他只負責調查，後來就離開，是鄉公所依照示範計畫的調查結果去申請的，他自己並不贊成現在的劃設辦法排除私有地，也提到在調查的時候根本不可能排除私有地。</p>
                              <p>但除此之外，若鄉公所沒有更改調查結果，向原民會提報申請的鄒族傳統領域就包含阿里山森林遊樂區。未來，是否真有機會讓部落享有諮商同意權、甚至森林遊樂區的利益共享？就看新政府要如何落實口中的轉型正義了。</p>
                              <figure>
                                <img src="./static/pnguu5.jpg"></img>
                                <figcaption>在立法還沒完備前，在原住民傳統領域開發都不需經過族人同意。</figcaption>
                              </figure>
                              <p>湯文賢也苦笑著說，就算調查、確立了傳統領域，部落內對這件事並沒有太熱情，「族人很清楚，就算確認了傳統領域的範圍，政府也沒有要還給我們，「我們只能不斷地說明為什麼這件事這麼重要，至少是一個起點」。</p>
                              <p>提到對傳統領域權的期待，不舞提到了她最愛的故事——山豬王與少女，故事述說山豬王與鄒族少女相戀的故事，雖然最後以悲劇終結，卻也彰顯過去大自然與人類的關係非常緊密。「希望國家能把土地還給真正珍愛土地的人，原住民就有機會跟大自然再度譜出戀曲吧」，她笑著說。</p>`,
              title: `<h1 class="section__title--invisible section__title">阿里山來吉部落：<br><span class="section--focus">人與自然的戀曲</span></h1>`,
              subtitles: `<p class="section__subtitle">短短的四百多年殖民政權，完全改變了原住民族的樣貌，土地也不斷地流失。日治時期，有人要在現在的阿里山森林遊樂區內開墾，還得跟原住民租地；國民政府來台以後卻直接蓋了一座門，把族人阻擋在外。回到部落自力更生的鄒族的不舞（Pu-u Akuyana）深信，與自然共處能力是原住民古老的基因，只要讓原住民有權利自主規劃運用傳統領域，一定能跟祖先們一樣，譜出美好的戀曲。</p>`// TODOs: TBA
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-4',
              descriptionModifier: 'left',
              theme: 'dark',
              hasMore: true,
              hasBackgroundPhoto: true,
              photoCredit: '攝影：簡信昌',
              moreCaption: '看新竹司馬庫斯部落的故事',
              moreTitle: `<h1>新竹司馬庫斯部落：<br>傳統領域比生命更重要</h1>`,
              moreSubtitles: `<p>下午四點半，造訪司馬庫斯的遊客紛紛集結到遊客中心前，準備參加導覽。今天的導覽員是拉互依（Lahwy），他是部落的第一個碩士，當年還把口試現場搬到部落，親身體驗他論文裡所寫的，畢業後便回到部落工作。</p>
                              <p>不過是一個小時前，拉互依還背著機器除草，也因此導覽解說時還可見到他的衣服上黏著雜草；導覽一結束，他又小跑步離開現場。讓我想起約訪拉互依時，他熱情地歡迎我們來到部落，但對敲定確切的時間點有些為難，「我們每天都有好多工作要做啊！」</p>
                              <figure class="portrait">
                                <img src="./static/smangus1.jpg"></img>
                                <figcaption>拉互依正在對來部落觀光的遊客進行導覽。</figcaption>
                              </figure>
                              <p>司馬庫斯部落是共有制，這裡的共有不是空泛的概念，包括了加入的所有成員的私有土地都成為公共資產，工作也統一安排，不管男女老少薪資都是兩萬元（除了 60 歲退休的老人，但每月有一萬兩千元的敬老津貼）。現在司馬庫斯一共有 58 個領薪水的員工，主要的收入來自觀光（約佔八成），支付薪水以後就作為部落的經營的基金，教育、結婚、生育都有補助，可以說每個孩子都是部落「養」大的，也強調年輕人畢業只要願意回來部落，一定會有工作做。</p>
                              <p>拉互依在導覽的時候笑說，「共享是古老的原住民精神，但祖先頂多只是共享獵物、農作物，我們分享的精神已經超越祖先了。」</p>
                              <p>除了觀光以外，族人的日常工作就是維護環境，因為傳統領域是部落共有的資產。除草、維修木棧道、清理道路，部落的族人將之視為自己的責任，「部落對傳統領域的概念，比生命更重要，傳統領域是我們祖先密不可分的環境，他就是靠這樣的環境來養育他的子女與生活的需要。」部落頭目馬賽（Masay）說。</p>
                              <p>司馬庫斯對部落傳統領域自有一套規劃。早期，他們也曾以販售水蜜桃維生，但後來卻因生態保育的選擇「自斷財源」。馬賽解釋，「原住民住的地區很多都是山坡地，政府給的保留地又不大。有的部落就一直挖、一直開發，都是在製造土石流的後遺症。我們的主軸是把土地復育，讓它更堅固、更安全，盡量種樹，如櫻花、楓香、杉木，也讓城市的朋友有一個可以來放鬆壓力的地方。」</p>
                              <figure>
                                <img src="./static/smangus2.jpg"></img>
                                <figcaption>觀光現在是司馬庫斯最主要的收入。</figcaption>
                              </figure>
                              <p>連打獵也有相關管制，部落劃設禁獵區，如巨木群、鴛鴦湖等核心區不能打獵。而因季節性動物多、或特殊節日需求，像聖誕節、感恩節，部落就會派年輕人去打幾隻山羌、飛鼠，供應部落基金、族人結婚的禮物等等。目前司馬庫斯正進入第二階段的禁獵期，總共十年的時間。</p>
                              <p>「剛開始推動野生動物保育，周邊的部落都在生氣。」馬賽苦笑，「都罵我們說，你們這個小小的部落是吃錯藥嗎？泰雅族的文化就是以打獵維生，怎麼可以這樣！但第一階段結束之後，山羌一直蔓延到他們家後面都出現，有人說在馬路邊都可以打到，大家都嚇到了。」</p>
                              <p>「我們部落的人才去解釋，就是因為司馬庫斯（實施野生動物）保護了幾年，才有這樣的成果。現在他們都知道了，還說『之前你們好好來說明就好了啊』！」馬賽深信，正確的事情只要堅持下去，會被其他人理解，甚至效仿。</p>
                              <p>但司馬庫斯也不是一開始就這樣。就像拉互依向導覽的聽眾介紹放在部落中心的木雕藝術，上頭有各式各樣的腳印，創作者就是現任的頭目馬賽。循著木雕上腳印前進的方向，一開始的腳型較大、腳印也深，象徵文明還沒來到以前開墾部落的先人。但緊接著的腳印卻步伐混亂。</p>
                              <p>馬賽回憶，1980年代，原本部落還靠種植香菇有微薄的收入，但當時中國偷渡的香菇盛行，讓部落漸漸無法生存，有超過一半的族人到新竹工業區工作，「經過一段時間，族人興奮地跑回來跟我們分享，趕快下山吧！外面每天都在賺錢！」還有族人對他說，「乾脆我們把部落所有的土地都賣掉，在工業區外面買房子，在那裡過一輩子吧！」</p>
                              <figure>
                                <img src="./static/smangus3.jpg"></img>
                                <figcaption>部落頭目馬賽說，保護傳統領域是司馬庫斯族人歷代傳承的重要使命。</figcaption>
                              </figure>
                              <p>當時司馬庫斯僅有八戶人家，好險，1995 年，司馬庫斯唯一的聯外道路開通了。但路帶來的不只是繁榮，馬賽說，當時八戶人家各走各的，為了巨木群帶來的觀光人潮，開始互相競爭，「做禮拜的時候，在教會裡頭唱歌好像很虔誠、很開心，離開了教會，在小小的路上連兄弟碰面都不打招呼⋯⋯。」</p>
                              <p>「大家都覺得氣氛非常糟糕。這時候，前任頭目（倚岕・蘇隆）帶領大家做了一個重要的決定。他說，我們為什麼不把祖先哪種共享、共榮的生活找回來？把部落所有的資產聚集在一起，一起來運用。」馬賽回憶，當時他們也看到很多的財團用買土地的方式介入原住民社會，「原本是主人的原住民，賣掉土地之後，變成了傭人。看到這樣的例子愈來愈多，我們毫不猶豫，（決定）不要走他們的後路。」</p>
                              <p>在混亂的腳印過後，木雕上刻著的是整齊劃一的步伐，有高跟鞋、皮鞋等各種鞋子的形狀，但方向卻一致向前，「代表司馬庫斯走出了自己的一條路。」拉互依說。</p>
                              <p>這樣特殊的機制與背景團結了族人，司馬庫斯甚至有台灣司法史上第一個承認原住民傳統領域權力的判案。 2005 年，颱風造成司馬庫斯唯一的聯外道路崩塌，部落召開特別會議，決定自行處理，將一棵橫倒在路中的的櫸木移至路邊。一個多月後，林務局林管處才將這棵貴重的巨木截成數段運下山，留下樹根在現場。部落認為剩下的殘幹可以拿來做木雕、建築，表達部落意象，派了三位年輕人去處理，回程卻遇到警察，依《森林法》竊盜罪移送偵辦。</p>
                              <p>司馬庫斯部落的族人主張原住民在傳統領域應有使用自然資源的權利，跟公部門論辯了數年，直至 2010 年，台灣高等法院做出更一審宣判，引用《原基法》精神，判決被告無罪。只可惜，後來櫸木事件對國家來說只停留在被告無罪的層次，沒有進一步地去思考與原住民共管、共治的可能。</p>
                              <p>等了 7 年，傳統領域劃設辦法終於公佈。馬賽先是肯定總統蔡英文的「道歉」，也表示只要傳統領域的概念完整（不分公私有），應該先把固有的傳統領域直接下放，而私有土地問題太過複雜，可以再慢慢處理。</p>
                              <p>但馬賽接著說，「有誠意道歉的政府，應該要有一個認知。對於原住民的傳統領域，應該毫不猶豫地還給原住民，讓原住民管理他自己的獵場、管理自然資源的運用。」</p>
                              <p>馬賽也補充，「櫸木事件的最後，國家雖然公告、承認了我們的傳統領域。但他們的版本跟我們的版本有極大的落差。不過，就算他們不接受，我們還是用我們的版本。」比起其他部落會碰到私有地開發案的爭議，司馬庫斯的傳統領域屬公有地與原保地，考驗的正是國家對於轉型正義的決心。</p>
                              <figure class="portrait">
                                <img src="./static/smangus4.jpg"></img>
                                <figcaption>離部落五公里遠的神木林是吸引觀光客的重要景點。</figcaption>
                              </figure>
                              <p>由於現行原住民傳統領域劃設辦法碰到公有地時，部落提出的申請範圍仍要獲得主管機關確認，被抗議者認為誠意不夠。記者事後採訪原民會土管處傳統領域科，吳科長表示，「所謂的『確認』並不是『同意』，涉及到各個公產機關，我們還是會讓他們表示意見。但只要部落提供相關佐證資料，我們對這個範圍不會有太大的意見，原則上都是尊重部落自主劃設的內容。假設公產機關的意見太強烈，我們會組成商議小組，邀請部落、民族代表、專家學者一起認定，不是單方面讓公產機關表示同不同意。」</p>
                              <p>但儘管司馬庫斯部落以共有制管理現有的傳統領域，連私有地（原住民保留地）都充公，還是有兩成的族人沒有參加，自己經營自己的民宿與露營區。馬賽強調，儘管這兩成的族人沒有參加合作社，也是部落的一份子。未來諮商同意權的行使，屬於部落的事務，他們不會被排除在外。他也提到，「過去這兩成的族人對政府的開發也不能接受，對保護土地、買賣土地的事都不會隨便。這樣的共識是很清楚的。」</p>
                              <p>司馬庫斯部落在傳統領域權的實踐上，花了十幾年走出一條新的路，也為台灣社會示範了其中一種轉型正義的未來。族人也還在努力，無論是生存、戮力執行守護傳統領域的責任、抑或是那兩成仍未加入合作社的族人，就像拉互依在導覽的最後感謝漢人觀光客的到來時說的：「路一定會愈來愈好的，只是時間長短的問題。」</p>`,
              title: `<h1 class="section__title--invisible section__title">新竹司馬庫斯部落：<br><span class="section--focus">傳統領域比生命更重要</span></h1>`,
              subtitles: `<p class="section__subtitle">司馬庫斯（Smangus）位在新竹尖石鄉海拔 1578 公尺的高山上，唯有一條聯外道路進出，自稱「上帝的部落」。這個地方不等政府施捨，用自己的力量實踐了《原基法》賦予原住民的權利，雖然司馬庫斯的故事有一定的特殊性，但在「把傳統領域還給原住民後會發生什麼事」的問題上，或許可以提供讀者一些想像。</p>`// TODOs: TBA
            },
            {
              dataAnchor: 'the-issues-and-solutions-on-traditional-indigenous-territories--section-5',
              descriptionModifier: 'space-around',
              theme: 'dark',
              issues: [
                {
                  id: '21-1',
                  title: `<h1 class="section__issue-title">「誰」可以畫傳統領域？</h1>`,
                  subtitle: `<p class="section__subtitle">要完整實踐原住民的知情同意權，有三個重要的元素。<span class="mobile-hide">目前「在哪裡劃」、「在什麼情形下可以行使權利」都已有相關法條，但「誰」可以劃傳統領域呢？劃設傳統領域的權利主體是誰？是什麼樣的組織？</span><span class="desktop-hide">⋯⋯</span></p>`,
                  content: `<p>雖然原民會已有規劃要推動「部落公法人」機制，但目前各方對它的功能有不同的想像跟期待，尚未完全定案。設置部落公法人是讓部落在體制內有法律上的「主體」，原住民族政策協進會執行長陳旻園指出，這是過去推動傳統領域的劃設、到處理所有權的《土海法》，都不斷碰到的問題，也就是土地要畫給誰？誰有權利主張？</p>
                            <p>「當然，集體意識強的族群比較會講話，但對大部分的族群跟部落來說，內部的討論機制都已經被現在的民意機關、組織架空了。」陳旻園指出，部落裡可能有民族議會、教會、協會等有代表性的組織，但在法律上只有行政機構跟民意代表有法源依據，這是為什麼目前看到的部落代表都是後者。</p>
                            <p>陳旻園指出，主體需要定義清楚，部落也需要政府協助培力。他舉劃設辦法為例，要先組一個劃設團隊、再提出範圍計畫、送到鄉公所、核定完之後再送到縣市政府⋯⋯，「程序之複雜，就像過去保留地的增擴編，持續了 20 幾年，還有好幾萬件卡在公部門過不去，這也會造成族人的消極，反正又過不了。」</p>
                            <p>「最好的方式，就是主動協助。這不就是主管機關要做的嗎？今天已經有這麼多部落自主宣告，那原民會做了什麼？你要去鼓勵、去幫忙，其他部落看到成效出來，就會覺得我也可以試試看。」陳旻園感嘆，本應該是照顧族人的部會，反而設下這麼高的門檻，像在打小孩給別人看。</p>
                            <p>但陳旻園也提醒，原住民也應該準備好，「如果部落沒有主體，還要等待政府的輔佐，等待政府劃一塊傳統領域給你，那跟過去的福利政策有什麼不同？對於社會大眾來說，也會面臨一個問題，原住民憑什麼？永遠就陷在這樣的爭議裡。」</p>`
                },
                {
                  id: '21-2',
                  title: `<h1 class="section__issue-title">《土海法》會是萬靈丹嗎？</h1>`,
                  subtitle: `<p class="section__subtitle">按照總統蔡英文的指示，傳統領域的劃設要「分階段」<span class="mobile-hide">實行，私有地要透過《原住民土地及海域法》來處理。過去三進三出立法院的《土海法》，這次過得了關嗎？</span><span class="desktop-hide">⋯⋯</span></p>`,
                  content: `<p>私有地被排除在傳統領域之外，除了諮商同意權行使範圍無法完整以外，原住民族也有土地再也拿不回來的焦慮。除了因殖民因素被侵佔的公有地、被騙或在不知情狀況簽下的放棄土地文件，樣態實在太多元。政大民族系副教授官大偉就舉例，尖石鄉的前山有一塊地，是日治時期要蓋軍用機場時，政府拿原住民傳統領域跟私人企業換地。他明明是原住民的傳統領域，但按照現在的法令，它就是被排除在外的私有地。</p>
                            <p>而原民會的態度是，這些爭議土地權屬的問題，要放到《土海法》來解決。目前原民會是依照總統蔡英文擔任原住民轉型正義委員會主席在 3 月 20 日做出來的結論，也就是傳統領域劃設辦法分階段實施，初期先排除私有地，並持續推動《原住民族土地與海域法》的立法。</p>
                            <p>《土海法》是根據《原基法》第 20 條，明訂「政府承認原住民族土地及自然資源權利」。但《土海法》由於牽涉太廣，加上是直接規範部落可以有集體使用土地的權利，過去三進三出立法院，都未能成功立法。</p>
                            <figure>
                                <img src="./static/issue2.jpg"></img>
                                <figcaption>反對者認為，連只有諮商同意權的傳統領域劃設辦法都有問題，何況是涉及所有權的土海法。</figcaption>
                            </figure>
                            <p>立委高潞・以用提到，卡關的原因是「各部會、主流社會都還不認識原住民」，原住民族政策協會執行長陳旻園也指出，去年在立法院的時候還被國民黨召委林益世質疑「為什麼拿這種分土地的法案來討論」，讓他感嘆，轉型正義在某些人心中被誤解得十分不堪。</p>
                            <p>那這次原民會哪來的自信，把它當作弭平爭議的關鍵？原民會傳統領域科吳科長指出，關鍵就在去年 8 月 1 日總統蔡英文跟原住民道歉。他提到，總統的政見跟過去有很大的不同，目前正依據政見與道歉文對《土海法》草案做通盤檢討。</p>
                            <p>但原住民族政策協會執行長陳旻園直言，連諮商同意權這麼低的權利，都被限縮在公有地上；土海法涉及的是土地返還、共管等高強度的權利，「怎麼可能會有放寬的機會」。</p>
                            <p>被問及《土海法》立法的時程，原民會表示未來要經過原轉會充分討論後，才進行後續法制作業（最新一次的原轉會因應蘭嶼核廢料調查報告出爐，主題聚焦在真相還原與補償上），雖然被列為優先法案，但實際的時程還無法預估。總統的道歉會是萬靈丹，讓所有的歧視與誤解都消失嗎？</p>`
                },
                {
                  id: '21-3',
                  title: `<h1 class="section__issue-title">一條回家的路？</h1>`,
                  subtitle: `<p class="section__subtitle">即使現有的傳統領域劃設辦法爭議被解決<span class="mobile-hide">，原住民真的就能在傳統領域上做自己想做的事了嗎？原住民回家的路，需要整個社會一起努力⋯⋯</span><span class="desktop-hide">&nbsp⋯⋯</span></p>`,
                  content: `<p>「原住民好像從出生就是違法的。」阿度蘭阿美斯文化協進會的總幹事蔡政良提到族人在鄰近海域捕魚的狀況，「海巡署先抓再說，部落族人就是先躲再說。原住民就是一直處在這種陰影下，不斷地躲、不斷地逃，這是累積了上百年，在原住民文化底蘊中一個很悲哀的層次⋯⋯。」</p>
                            <p>原住民傳統領域的完整劃設，是為原住民族開了一條回家的路；但原住民要能真正回到自己的家園、做自己想做的事，還有一段路。擋在他們面前的，至少有《礦業法》、《森林法》、《野生動物保護法》、《漁業法》、《槍砲彈藥管制條例》⋯⋯，《原基法》保障原住民權益的精神，碰到其他主管機關的法律，很少打勝仗。</p>
                            <p>雖然相關部會設計了一套「事先報備」的制度，讓原住民能夠因傳統文化、祭儀等非營利目的打獵，但事先報備本身就與打獵文化違背。如布農族的獵人若要上山取獵物，不能大聲張揚，否則就無法獲得山神的贈禮；獵槍還只能用不安全的「土製獵槍」，不合時宜的法律甚至危害到獵人的生命危險。而從 2003 年到現在，已有 382 位原住民因為日常狩獵被判刑，刑期最重是 3 年 6 個月。</p>
                            <p>又如同《礦業法》中地主就算不願意、礦業權人只要提存一筆錢就能逕自開發的相關法條，讓族人就算經過所有權確認的法律長路，拿到了所有權狀，面對土地被礦場霸佔仍莫可奈何。</p>
                            <figure>
                                <img src="./static/issue3.jpg"></img>
                                <figcaption>原住民的轉型正義不只有傳統領域這一項。</figcaption>
                            </figure>
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
              title: `<h1 class="section__title--invisible section__title section--gray">想繼續討論原住民議題嗎？</h1>`,
              subtitles: `<div class="li--inside mobile-marginless">
                            <p class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: .7s"><a style="text-decoration: none; border-bottom: 1px solid gray; color: gray" target="_blank" href="https://www.facebook.com/IndigenousTransformativeJusticeTW/?fref=ts" onclick="ga('send', 'event', 'projects', 'click', '22 hyper1', { nonInteraction: false })">參加原轉小教室，去街頭陪伴還在抗議的原住民</a></p>
                            <p class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: 1.4s"><a style="text-decoration: none; border-bottom: 1px solid gray; color: gray" target="_blank" href="https://www.cet-taiwan.org/node/3030" onclick="ga('send', 'event', 'projects', 'click', '22 hyper2', { nonInteraction: false })">參加環原台灣論壇，瞭解更多原住民族權利</a></p>
                            <p class="section__subtitle list-margin-normal section--gray self-animate self-animate--invisible" style="animation-delay: 2.1s"><a style="text-decoration: none; border-bottom: 1px solid gray; color: gray" target="_blank" href="https://www.facebook.com/IndigenousYouthFront/?fref=ts" onclick="ga('send', 'event', 'projects', 'click', '22 hyper3', { nonInteraction: false })">跟原住民族青年陣線一起監督蔡英文的原民政策（平台即將在 8/3 上線！）</a></p>
                          </div class="li--inside">`
            }
          ]
        },
        {
          dataStoryId: 'ending-page',
          dataStoryTitle: '結束頁',
          sections: [
            {
              dataAnchor: 'ending-page',
              descriptionModifier: 'center',
              theme: 'dark',
              hasMore: false,
              title: `<h1 class="section__title--invisible section__title"></h1>`,
              subtitles: `<p class="section__subtitle mobile-hide" style="margin:0; padding-top: 10%">文字採訪：李又如&nbsp&nbsp&nbsp&nbsp&nbsp攝影：林俊耀、簡信昌、鐘聖雄、楊子磊&nbsp&nbsp&nbsp&nbsp&nbsp影音：楊仁翔、陳岳威</p>
                          <p class="section__subtitle mobile-hide" style="margin:0">視覺設計：陳怡蒨&nbsp&nbsp&nbsp&nbsp&nbsp網頁製作：熊凱文</p>
                          <p class="section__subtitle mobile-hide" style="margin:0">鏡傳媒 MirrorMedia</p>
                          <p class="section__subtitle mobile-hide" style="margin:0">2017.08.01</p>
                          <p class="section__subtitle desktop-hide" style="margin:0; padding-top: 10%">文字採訪：李又如</p>
                          <p class="section__subtitle desktop-hide" style="margin:0">攝影：林俊耀、簡信昌、鐘聖雄、楊子磊</p>
                          <p class="section__subtitle desktop-hide" style="margin:0">影音：楊仁翔、陳岳威</p>
                          <p class="section__subtitle desktop-hide" style="margin:0">視覺設計：陳怡蒨</p>
                          <p class="section__subtitle desktop-hide" style="margin:0">網頁製作：熊凱文</p>
                          <p class="section__subtitle desktop-hide" style="margin:0">鏡傳媒 MirrorMedia</p>
                          <p class="section__subtitle desktop-hide" style="margin:0">2017.08.01</p>`
            }
          ]
        }
      ]
    }
  },
  methods: {
    readMore () {
      /* eslint-disable no-undef */
      $.fn.fullpage.setAllowScrolling(false)
      this.currentSection.showMoreContent = !this.currentSection.showMoreContent
      $('.hamburger').hasClass('is-active') ? $('.hamburger').removeClass('is-active') : $('.hamburger').addClass('is-active')
      $('.container-more__caption').addClass('container-more__caption--invisible')
      $('.left.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
      if (this.currentSection.moreCaption === '讓原住民保有傳統領域沒那麼可怕') {
        ga('send', 'event', 'projects', 'click', '13 info', { nonInteraction: false })
      } else if (this.currentSection.moreCaption === '現行劃設辦法會造成立即的侵害！') {
        ga('send', 'event', 'projects', 'click', '14 info', { nonInteraction: false })
      } else if (this.currentSection.moreCaption === '看台東都蘭部落的故事') {
        ga('send', 'event', 'projects', 'click', '17 info', { nonInteraction: false })
      } else if (this.currentSection.moreCaption === '看台東卡大地布部落的故事') {
        ga('send', 'event', 'projects', 'click', '18 info', { nonInteraction: false })
      } else if (this.currentSection.moreCaption === '看來吉不舞的故事') {
        ga('send', 'event', 'projects', 'click', '19 info', { nonInteraction: false })
      } else if (this.currentSection.moreCaption === '看新竹司馬庫斯部落的故事') {
        ga('send', 'event', 'projects', 'click', '20 info', { nonInteraction: false })
      }
    },
    closeMore () {
      this.currentSection.showMoreContent = false
      $('.left.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    readIssues (issue) {
      $.fn.fullpage.setAllowScrolling(false)
      this.currentSection.currentIssueId = issue.id
      this.currentSection.currentIssueContent = issue.content
      this.currentSection.showIssueContent = true
      $('.bottom.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
      if (this.currentSection.currentIssueId === '21-1') {
        ga('send', 'event', 'projects', 'click', '21-1 info', { nonInteraction: false })
      } else if (this.currentSection.currentIssueId === '21-2') {
        ga('send', 'event', 'projects', 'click', '21-2 info', { nonInteraction: false })
      } else if (this.currentSection.currentIssueId === '21-3') {
        ga('send', 'event', 'projects', 'click', '21-3 info', { nonInteraction: false })
      }
    },
    closeIssues () {
      this.currentSection.showIssueContent = false
      $('.bottom.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    moveTo (story, dataStoryIndex) {
      /* eslint-disable no-undef */
      // $.fn.fullpage.setAllowScrolling(false)
      this.currentStory.dataStoryIndex = dataStoryIndex
      // $('.sidebar').sidebar('hide')
      $('#app').animateCss('fadeOut', 'fadeIn', story)
      ga('send', 'event', 'projects', 'click', `nav${dataStoryIndex}`, { nonInteraction: false })
    },
    toggleSidebar () {
      $.fn.fullpage.setAllowScrolling(false)
      $('.right.sidebar')
        .sidebar('setting', 'transition', 'overlay')
        .sidebar('toggle')
    },
    closeSidebar () {
      $('.ui.right.sidebar')
      .sidebar('hide')
    },
    changeTheme (theme) {
      this.currentSection.theme = theme
    },
    changeCurrentStory (loadedSection, loadedDOM) {
      this.currentStory.dataStoryId = loadedDOM.attr('data-story-id')
      this.currentStory.dataStoryTitle = loadedDOM.attr('data-story-title')
      this.currentStory.dataStoryIndex = Number(loadedDOM.attr('data-story-index'))
      this.currentSection.dataSectionIndex = Number(loadedDOM.attr('data-section-index'))
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
    const windowWidth = $(window).width()

    // Landing page background video and poster handler
    if (windowWidth > 1100) {
      document.getElementById('bgvid-source').setAttribute('src', './static/landing-page.mp4')
    } else if (windowWidth > 767) {
      document.getElementById('bgvid').style.left = '7%'
      document.getElementById('bgvid').setAttribute('poster', 'https://www.mirrormedia.mg/projects/20170801aboriginal/ogimage.jpg')
      document.getElementById('bgvid-source').setAttribute('src', '')
    } else if (windowWidth > 413) {
      document.getElementById('bgvid').style.left = '5%'
      document.getElementById('bgvid').setAttribute('poster', 'https://www.mirrormedia.mg/projects/20170801aboriginal/ogimage.jpg')
      document.getElementById('bgvid-source').setAttribute('src', '')
    } else if (windowWidth > 374) {
      document.getElementById('bgvid').style.left = '-15%'
      document.getElementById('bgvid').setAttribute('poster', 'https://www.mirrormedia.mg/projects/20170801aboriginal/ogimage.jpg')
      document.getElementById('bgvid-source').setAttribute('src', '')
    } else {
      document.getElementById('bgvid').style.left = '-30%'
      document.getElementById('bgvid').setAttribute('poster', 'https://www.mirrormedia.mg/projects/20170801aboriginal/ogimage.jpg')
      document.getElementById('bgvid-source').setAttribute('src', '')
    }

    // $(document).ready(() => {
    $('#fullpage').fullpage({
      // menu: '#menu',
      // anchors: ['11', '12', '13', '21', '22', '23'],
      // sectionsColor: ['#C63D0F', '#1BBC9B', '#7E8F7C', '#C63D0F', '#1BBC9B', '#7E8F7C'],
      lockAnchors: true,
      animateAnchor: false,
      // scrollBar: true,
      normalScrollElements: '#readmore, #issues-content',
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
          vm.animateVisibleSelector(loadedSection, ['hamburger--background-dark', 'container-more__caption'], true)
        }
        if (sections[index - 1].issues) {
          vm.animateVisibleSelector(loadedSection, ['hamburger--background-dark', 'vertical-line', 'space-around__container'], true)
        }

        vm.changeCurrentStory(sections[index - 1], loadedSection)
        if (anchorLink === 'what-is-going-on-in-KBlvd--section-1') {
          vm.animateCountUp()
        }

        if (anchorLink === 'landing-page') {
          $('#bgvid')[0].play()
        }

        if (anchorLink === 'ending-page') {
          $.fn.fullpage.setAutoScrolling(false)
          $.fn.fullpage.setFitToSection(false)
        }

        if (!vm.haveSeen[index]) {
          let interact
          index > 9 ? interact = false : interact = true
          vm.haveSeen[index] = true
          if (index !== 1) {
            ga('send', 'event', 'projects', 'scroll', `scroll to ${index}`, { nonInteraction: interact })
          }
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
          vm.animateVisibleSelector(leavingSection, ['hamburger--background-dark', 'container-more__caption'], false)
        }
        if (sections[index - 1].issues) {
          vm.animateVisibleSelector(leavingSection, ['hamburger--background-dark', 'vertical-line', 'space-around__container'], false)
        }
        vm.changeTheme(sections[nextIndex - 1].theme)
        vm.storyOnChangeHandler(leavingSection, direction)
        // vm.backgroundThemeOnChangeHandler(index, nextIndex)

        // Folding left sidebar when scrolling outside of sidebar
        if (vm.currentSection.showMoreContent) {
          $('.left.sidebar')
          .sidebar('setting', 'transition', 'overlay')
          .sidebar('toggle')
        }
        if (vm.currentSection.showIssueContent) {
          $('.bottom.sidebar')
          .sidebar('setting', 'transition', 'overlay')
          .sidebar('toggle')
        }

        // Hiding bottom and left sidebars for elimating empty space remain on screen, may cause a flick on Safari ?
        if (nextIndex === sections.length) {
          document.querySelector('.ui.bottom.sidebar').style.display = 'none'
          document.querySelector('.ui.right.sidebar').style.display = 'none'
        } else if (index === sections.length) {
          $.fn.fullpage.setAutoScrolling(true)
          $.fn.fullpage.setFitToSection(true)
          $.fn.fullpage.fitToSection()
          document.querySelector('.ui.bottom.sidebar').style.display = 'block'
          document.querySelector('.ui.right.sidebar').style.display = 'block'
        }
      }
    })
    // })

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
      // onChange () {
      //   vm.sidebarHasToggle = !vm.sidebarHasToggle
      // },
      onVisible () {
        vm.sidebarHasToggle = true
        $('.ui.left.sidebar')
          .sidebar('hide')
      },
      onHide () {
        vm.sidebarHasToggle = false
      },
      transition: 'overlay',
      mobileTransition: 'overlay',
      context: '#app'
    })
    $('#readmore')
    .sidebar({
      // onVisible () {
      //   $.fn.fullpage.setAllowScrolling(false)
      // },
      onHidden () {
        $.fn.fullpage.setAllowScrolling(true)
        $('.container-more__caption').removeClass('container-more__caption--invisible')
        this.scrollTop = 0
      },
      onHide () {
        vm.currentSection.showMoreContent = false
        $('.hamburger').removeClass('is-active')
        // $('.morevid')[0].pause()
        if (this.getElementsByClassName('morevid')[0]) {
          this.getElementsByClassName('morevid')[0].pause()
        }
      },
      transition: 'overlay',
      mobileTransition: 'overlay',
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
p
  text-align justify
#app
  animation-duration .5s
  &.pushable
    overflow-x initial

#bgvid
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

#landing-image
  width 30%
  opacity 1
  animation-name landing-image
  animation-duration 2s
  animation-delay 14s
  animation-fill-mode forwards

@keyframes landing-image
  0%
    opacity 1
  100%
    opacity 0

.right.sidebar
  z-index 1002 !important
  width 20% !important
  border none !important
  .item
    height 25% !important
    box-sizing border-box
    border-top 1px solid #142d64 !important
    display flex !important
    justify-content  center !important
    align-items center !important
    font-size 20px
    color #142d64 !important
    font-weight 400 !important
    &.active
      background-color #142d64 !important
      color white !important
.custom-dimmed
  // pointer-events auto
  position absolute
  top 0
  left 0
  background black
  width 100%
  height 100vh
  z-index 1001
  opacity 0
  &--visible
    animation-name fadeInDimmed
    animation-duration .7s
    animation-delay 0s
    animation-fill-mode forwards
  &--invisible
    animation-name fadeOutDimmed
    animation-duration .7s
    animation-delay 0s
    animation-fill-mode forwards
@keyframes fadeInDimmed
  0%
    opacity 0
  100%
    opacity .3
@keyframes fadeOutDimmed
  0%
    opacity .3
  100%
    opacity 0


.left.sidebar
  width 50% !important
  background-color white
  padding-top 40px
  padding-bottom 40px
  padding-right 70px
  padding-left 70px
  box-sizing border-box
.ui.bottom.sidebar
  width 60% !important
  height 80% !important
  background-color white
  padding-top 40px
  padding-bottom 40px
  padding-right 70px
  padding-left 70px
  box-sizing border-box
  margin 0% 20% 0% 20%!important
.ui.visible.bottom.overlay.sidebar
  transform translate3d(0, -10%, 0) !important
.close-issue-btn
  position absolute
  bottom 0
  right 0
  margin-bottom calc(5% - 30px) !important
  margin-right calc(20% - 30px) !important
  // z-index 1000

.container-more
  position absolute
  left 50px
  bottom 30px
  z-index 101
  display flex
  justify-content center
  align-items center
  &__caption
    color white
    font-size 20px
    line-height 50px
    margin-left 10px
    opacity 0
    animation-name fadeIn
    animation-duration .7s
    animation-delay 1.5s
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
    font-size 22px
    line-height 40px
  span
    font-size 22px
  table, th, td
    line-height 1.5
    border 1px solid #bdbdbd
    border-collapse collapse
    font-size 22px
    padding 10px
  u
    text-decoration none
    font-weight bold
    color black
    // background-image linear-gradient(to right, #333 50%, rgba(255, 255, 255, 0) 0%)
    // background-position bottom
    // background-size 5px 2px
    // background-repeat repeat-x
    // padding-bottom .5%
  td:first-child
    width 15%

  // .video-container
  //   margin 5% 0
  //   position relative
  //   padding-bottom 56.25%
  //   padding-top 30px
  //   height 0 
  //   overflow hidden
  // .video-container iframe, .video-container object, .video-container embed
  //   position absolute
  //   top 0
  //   left 0
  //   width 100%
  //   height 100%
  .morevid
    margin 5% 0px 5% 0px
  figure
    img
      width 100%
      // height 500px
      // max-height 50vh
    padding 2% 0px 2% 0px
    margin 0
    // margin 0
    // position relative
    // padding-bottom 75.25%
    // & + p
    //   margin-top 5%
    // &[class="captionless"]
    //   padding-bottom 70.25%
    // &[class="portrait"]
    //   padding-bottom 100%
    figcaption
      // position absolute
      // bottom 0
      // font-size 20px
    .readmore__img
      // position absolute
      width 100%
      height 100%
      background-size cover
      background-position center center
      background-repeat no-repeat

  .portrait
    width 70%
    padding 2% 0px 2% 0px
    position relative
    left: 50%;  
    transform: translateX(-50%);
    .readmore__img
      height 96%

#issues-content
  figure
    max-height 100vh
    padding 5% 0

.section
  // Add for performance issues
  will-change transform
  transform translate3d(0, 0, 0)

  // background-size auto 100%
  background-size cover
  background-repeat no-repeat
  background-position center center

  // Ladning page
  &[data-anchor="landing-page"]
    .section__description--center
      background-color rgba(0, 0, 0, .5)
      animation landing-page
      animation-duration 2s
      animation-delay 14s
      animation-fill-mode forwards
  @keyframes landing-page
    0%
      background-color rgba(0, 0, 0, .5)
    100%
      background-color rgba(0, 0, 0, 0)
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
    background-position center 20%
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
    // background-position center 0%
  &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-5"]
    background-color #142d64
  // ending page
  &[data-anchor="ending-page"]
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
      // background url('static/test.svg')
      &-wide
        justify-content center
        align-items flex-start
        text-align left
        max-width 64%
        -webkit-backface-visibility hidden
        background linear-gradient(to left, rgba(0, 0, 0, 0), rgba(0, 0, 0, 1))
    &--center
      p
        text-align center
      justify-content center
      align-items center
      text-align center
      padding 0px 15%
      max-width 100%
      // &-wide
      //   p
      //     text-align center
      //   justify-content center
      //   align-items center
      //   text-align center
      //   padding 0
      //   max-width 100%
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

  .photo-credit
    position absolute
    bottom 49px
    right 50px
    font-size 14px
    color white
    text-shadow 2px 2px 8px black
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

@media (max-width: 1600px)
  .right.sidebar
    width 22% !important
    .item
      font-size calc(20px/1.4)
  .left.sidebar
    padding-top calc(40px/1.4)
    padding-bottom calc(40px/1.4)
    padding-right calc(70px/1.4)
    padding-left calc(70px/1.4)
  .ui.bottom.sidebar
    width 70% !important
    margin 0% 15% 0% 15%!important
    padding-top calc(40px/1.4)
    padding-bottom calc(40px/1.4)
    padding-right calc(70px/1.4)
    padding-left calc(70px/1.4)
  .close-issue-btn
    margin-bottom calc(5% - 30px) !important
    margin-right calc(15% - 30px) !important
  .container-more
    &__caption
      font-size calc(20px/1.4)
  #readmore, #issues-content
    h1
      font-size calc(42px/1.4)
    p
      font-size calc(22px/1.4)
      line-height calc(40px/1.4)
    span
      font-size calc(22px/1.4)
    table, th, td
      font-size calc(22px/1.4)
      padding 5px
    figure
      // height calc(500px/1)
      figcaption
        font-size calc(20px/1.4) !important
    .portrait
      // height calc(800px/1.4)
  .section
    .photo-credit
      font-size calc(20px/1.4)

@media (max-width: 1439px)
  .right.sidebar
    width 22% !important
    .item
      font-size calc(20px/1.6)
  .left.sidebar
    padding-top calc(40px/1.6)
    padding-bottom calc(40px/1.6)
    padding-right calc(70px/1.6)
    padding-left calc(70px/1.6)
  .ui.bottom.sidebar
    width 70% !important
    margin 0% 15% 0% 15%!important
    padding-top calc(40px/1.6)
    padding-bottom calc(40px/1.6)
    padding-right calc(70px/1.6)
    padding-left calc(70px/1.6)
  .close-issue-btn
    margin-bottom calc(5% - 30px) !important
    margin-right calc(15% - 30px) !important
  .container-more
    &__caption
      font-size calc(20px/1.6)
  #readmore, #issues-content
    h1
      font-size calc(42px/1.6)
    p
      font-size calc(22px/1.6)
      line-height calc(40px/1.6)
    span
      font-size calc(22px/1.6)
    table, th, td
      font-size calc(22px/1.6)
      padding 5px
    figure
      // height calc(500px/1.2)
      figcaption
        font-size calc(20px/1.6) !important
  .section
    .photo-credit
      font-size calc(20px/1.6)

@media (max-width: 1279px)
  .right.sidebar
    .item
      font-size calc(20px/1.7)
  .left.sidebar
    padding-top calc(40px/1.7)
    padding-bottom calc(40px/1.7)
    padding-right calc(70px/1.7)
    padding-left calc(70px/1.7)
  .ui.bottom.sidebar
    padding-top calc(40px/1.7)
    padding-bottom calc(40px/1.7)
    padding-right calc(70px/1.7)
    padding-left calc(70px/1.7)
  .container-more
    bottom 20px
    &__caption
      font-size calc(20px/1.7)
  .hamburger.close-more-btn
    bottom 20px !important
  #readmore, #issues-content
    h1
      font-size calc(42px/1.7)
    p
      font-size calc(22px/1.7)
      line-height calc(40px/1.7)
    span
      font-size calc(22px/1.7)
    table, th, td
      font-size calc(22px/1.7)
    figure
      figcaption
        font-size calc(20px/1.7) !important
  .section
    .photo-credit
      font-size calc(20px/1.7)
      bottom 43px

// For IPad
@media (max-width: 1100px)
  #landing-image
    width 70%
  .right.sidebar
    width 80% !important
    .item
      font-size 25px
  .left.sidebar
    width 80% !important
    overflow scroll
    padding 10px 30px
  .ui.bottom.sidebar
    width 100% !important
    height 100% !important
    background-color white
    padding 5px 15px
    padding-bottom 20px
    box-sizing border-box
    margin 0!important
  .ui.visible.bottom.overlay.sidebar
    transform translate3d(0, 0, 0) !important
  .close-issue-btn
    bottom 5px !important
    right 5px !important
    margin-bottom 0 !important
    margin-right 0 !important
    box-shadow 2px 2px 10px 1px gray !important
  .container-more
    left 10%
    bottom 10%
    &__caption
      font-size calc(20px/1.2)
      line-height 45px
  .hamburger.close-more-btn
    bottom 10% !important
  #readmore, #issues-content
    h1
      font-size calc(42px/1.2)
      font-weight 500
    p
      font-size calc(24px/1.2)
      line-height calc(44px/1.2)
    span
      font-size calc(24px/1.2)
    u
      padding-bottom 1%
    .morevid
      margin 5% 0px
    figure
      padding 5% 0px 5% 0px
      figcaption
        font-size calc(20px/1.2) !important
    .portrait
      width 100%

  .section
    &[data-anchor="what-is-going-on-in-KBlvd--section-3"]
      background-position 40% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-1"]
      background-position 60% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-3"]
      background-position 80% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-4"]
      background-position 100% center
    &__description
      padding 10%
      &--left
        max-width 100%
        background rgba(0, 0, 0, .5)
      &--left-wide
        justify-content flex-start
        padding-top 18%
        max-width 100%
      &--center
        p
          text-align left
        text-align left
        br
          display none
          &.remain
            display block 
      &--space-around
        padding 0
        justify-content flex-end

    .taiwan-line
      height 38%
      right 30%
    .taiwan-color
      height 38%
      right 30%
    .photo-credit
      font-size calc(14px/1)
      right 15px
      bottom 28px
    &[data-anchor="what-is-traditional-indigenous-territories--section-2"]
      .photo-credit
        bottom 15px

// For Iphone 6+
@media (max-width: 767px)
  .right.sidebar
    width 80% !important
    .item
      font-size 25px
  .left.sidebar
    width 80% !important
    overflow scroll
    padding 10px 20px
  .ui.bottom.sidebar
    width 100% !important
    height 100% !important
    background-color white
    padding 5px 15px
    padding-bottom 20px
    box-sizing border-box
    margin 0!important
  .ui.visible.bottom.overlay.sidebar
    transform translate3d(0, 0, 0) !important
  .close-issue-btn
    bottom 5px !important
    right 5px !important
    margin-bottom 0 !important
    margin-right 0 !important
    box-shadow 2px 2px 10px 1px gray !important
  .container-more
    left 15px
    bottom 15px
    &__caption
      font-size calc(20px/1.2)
      line-height 45px
  .hamburger.close-more-btn
    bottom 20px !important
  #readmore, #issues-content
    h1
      font-size calc(42px/1.6)
      font-weight 500
    p
      font-size calc(24px/1.2)
      line-height calc(44px/1.7)
    span
      font-size calc(24px/1.2)
    u
      padding-bottom 1%
    .morevid
      margin 5% 0px
    figure
      padding 5% 0px 5% 0px
      figcaption
        font-size calc(20px/1.2) !important
    // .portrait
    //   img
    //     width 100%

  .section
    &[data-anchor="what-is-going-on-in-KBlvd--section-3"]
      background-position 40% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-1"]
      background-position 60% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-3"]
      background-position 80% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-4"]
      background-position 100% center
    &__description
      padding 15px
      &--left
        max-width 100%
        background rgba(0, 0, 0, .5)
      &--left-wide
        justify-content flex-start
        padding-top 18%
        max-width 100%
      &--center
        p
          text-align left
        text-align left
        br
          display none
          &.remain
            display block 
      &--space-around
        padding 0
        justify-content flex-end

    .taiwan-line
      height 38%
      right 30%
    .taiwan-color
      height 38%
      right 30%
    .photo-credit
      font-size calc(14px/1.2)
      right 15px
      bottom 28px
    &[data-anchor="what-is-traditional-indigenous-territories--section-2"]
      .photo-credit
        bottom 15px

// For Iphone 6
@media (max-width: 413px)
  .right.sidebar
    width 80% !important
    .item
      font-size 22px
  .left.sidebar
    width 80% !important
    overflow scroll
    padding 10px 20px
  .ui.bottom.sidebar
    width 100% !important
    height 100% !important
    background-color white
    padding 5px 15px
    padding-bottom 20px
    box-sizing border-box
    margin 0!important
  .ui.visible.bottom.overlay.sidebar
    transform translate3d(0, 0, 0) !important
  .close-issue-btn
    bottom 5px !important
    right 5px !important
    margin-bottom 0 !important
    margin-right 0 !important
    box-shadow 2px 2px 10px 1px gray !important
  .container-more
    left 15px
    bottom 15px
    &__caption
      font-size calc(20px/1.4)
      line-height 45px
  #readmore, #issues-content
    h1
      font-size calc(42px/1.8)
      font-weight 500
    p
      font-size calc(24px/1.4)
      line-height calc(44px/1.9)
    span
      font-size calc(24px/1.4)
    u
      padding-bottom 1%
    .morevid
      margin 5% 0px
    figure
      padding 5% 0px 5% 0px
      figcaption
        font-size calc(20px/1.4) !important
    // .portrait
    //   img
    //     width 100%

  .section
    &[data-anchor="what-is-going-on-in-KBlvd--section-3"]
      background-position 40% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-3"]
      background-position 80% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-4"]
      background-position 100% center
    &__description
      padding 15px
      &--left
        max-width 100%
        background rgba(0, 0, 0, .5)
      &--left-wide
        justify-content flex-start
        padding-top 18%
        max-width 100%
      &--center
        p
          text-align left
        text-align left
        br
          display none
          &.remain
            display block 
      &--space-around
        padding 0
        justify-content flex-end

    .taiwan-line
      height 38%
      right 30%
    .taiwan-color
      height 38%
      right 30%
    .photo-credit
      font-size calc(14px/1.4)
      right 15px
      bottom 29px
    &[data-anchor="what-is-traditional-indigenous-territories--section-2"]
      .photo-credit
        bottom 15px
        
// For IPhone 5/5S/SE
@media (max-width: 374px)
  .right.sidebar
    width 80% !important
    .item
      font-size 20px
  .left.sidebar
    width 80% !important
    overflow scroll
    padding 10px 20px
  .ui.bottom.sidebar
    width 100% !important
    height 100% !important
    background-color white
    padding 5px 15px
    padding-bottom 20px
    box-sizing border-box
    margin 0!important
  .ui.visible.bottom.overlay.sidebar
    transform translate3d(0, 0, 0) !important
  .close-issue-btn
    bottom 5px !important
    right 5px !important
    margin-bottom 0 !important
    margin-right 0 !important
    box-shadow 2px 2px 10px 1px gray !important
  .container-more
    left 15px
    bottom 15px
    &__caption
      font-size calc(20px/1.7)
      line-height 45px
  #readmore, #issues-content
    h1
      font-size calc(42px/2.1)
      font-weight 500
    p
      font-size calc(24px/1.7)
      line-height calc(44px/2.2)
    span
      font-size calc(24px/1.7)
    u
      padding-bottom 1%
    .morevid
      margin 5% 0px
    figure
      padding 5% 0px 5% 0px
      figcaption
        font-size calc(20px/1.7) !important
    // .portrait
    //   img
    //     width 100%

  .section
    &[data-anchor="what-is-going-on-in-KBlvd--section-3"]
      background-position 40% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-3"]
      background-position 80% center
    &[data-anchor="the-issues-and-solutions-on-traditional-indigenous-territories--section-4"]
      background-position 100% center
    &__description
      padding 15px
      &--left
        max-width 100%
        background rgba(0, 0, 0, .5)
      &--left-wide
        justify-content flex-start
        padding-top 18%
        max-width 100%
      &--center
        p
          text-align left
        text-align left
        br
          display none
          &.remain
            display none
      &--space-around
        padding 0
        justify-content flex-end

    .taiwan-line
      height 40%
      right 30%
    .taiwan-color
      height 40%
      right 30%
    .photo-credit
      font-size calc(14px/1.7)
      right 15px
      bottom 30px
    &[data-anchor="what-is-traditional-indigenous-territories--section-2"]
      .photo-credit
        bottom 15px
</style>
