<template>
  <div class="related-posts-container">
    <p class="container-title">相關文章</p>
    <swiper :options="swiperOption" class="related-posts swiper-box"> 
      <swiper-slide class="swiper-item">
        <a class="related-posts__post" href="https://www.mirrormedia.mg/story/20170630pol002/" target="_blank">
          <img class="related-posts__leading-img" src="https://storage.googleapis.com/mirrormedia-files/assets/images/20170630102147-bcf487088429558fc4a3bcbf3d40f492-desktop.jpg"></img>
          <div class="related-posts__content-container">
            <p class="related-posts__title">【一鏡到底】凱道流浪記　巴奈專訪之一</p>
            <p class="related-posts__description">導演齊柏林逝世讓延燒數十年的亞泥爭議重現；金曲獎典禮上不少歌手高舉標語聲援原住民權益；70...</p>
          </div>
        </a>
      </swiper-slide>
      <swiper-slide class="swiper-item">
        <a class="related-posts__post" href="https://www.mirrormedia.mg/story/20170630pol003/" target="_blank">
          <img class="related-posts__leading-img" src="https://storage.googleapis.com/mirrormedia-files/assets/images/20170630102157-a3f676d644dcc44e02b6e8d8eddacb6f-desktop.jpg"></img>
          <div class="related-posts__content-container">
            <p class="related-posts__title">【一鏡到底】受傷與流浪　巴奈專訪之二</p>
            <p class="related-posts__description">談起原生家庭，她低聲說：「我的家庭很破碎…」凱道上綠燈亮起，引擎聲屢屢中斷訪談。父親是卡車...</p>
          </div>
        </a>
      </swiper-slide>
      <swiper-slide class="swiper-item">
        <a class="related-posts__post" href="https://www.mirrormedia.mg/story/20170630pol004/" target="_blank">
          <img class="related-posts__leading-img" src="https://storage.googleapis.com/mirrormedia-files/assets/images/20170630102232-b8c0f4012dbfeb51d2207095ae3f6440-desktop.jpg"></img>
          <div class="related-posts__content-container">
            <p class="related-posts__title">【一鏡到底】鄉愁與憂愁　巴奈專訪之三</p>
            <p class="related-posts__description">她回憶小時候，「洗溫泉不用花錢，只要鋤頭挖一挖，石頭疊一疊，把溪水流進來、溫度調好，就可以...</p>
          </div>    
        </a>
      </swiper-slide>
      <div class="swiper-button-prev" slot="button-prev" v-if="swiperOption.slidesPerView === 1"></div>
      <div class="swiper-button-next" slot="button-next" v-if="swiperOption.slidesPerView === 1"></div>
    </swiper>
  </div>
</template>
<script>
import Swiper from 'vue-awesome-swiper/swiper.vue'
import SwiperSlide from 'vue-awesome-swiper/slide.vue'
export default {
  components: {
    Swiper,
    SwiperSlide
  },
  computed: {
    swiperOption () {
      return {
        pagination: '.swiper-pagination',
        direction: 'horizontal',
        slidesPerView: this.responsiveSlidesPerView(),
        paginationClickable: true,
        // spaceBetween: 30,
        // mousewheelControl: true,
        loop: this.responsiveSlidesPerView() === 1,
        nextButton: '.swiper-button-next',
        prevButton: '.swiper-button-prev',
        simulateTouch: this.responsiveSlidesPerView() === 1
      }
    }
  },
  methods: {
    getWindowWidth () {
      let w = window
      let d = document
      let e = d.documentElement
      let g = d.getElementsByTagName('body')[0]
      let x = w.innerWidth || e.clientWidth || g.clientWidth
      return x
    },
    responsiveSlidesPerView () {
      if (this.getWindowWidth() > 767) {
        return 3
      } else {
        return 1
      }
    }
  }
}
</script>
<style lang="stylus">
// desktopWidth = 100%
// desktopHeight = desktopWidth*0.4
.related-posts-container
  width 80%
  height auto
  margin 5% 0
  margin-bottom 0
  // padding 40px 10px 0px 10px
  display flex
  flex-direction column
  justify-content center
  align-items center
  .container-title
    margin 0px 0px 24px 0px
    font-size 24px
    color white
    text-align center
  .related-posts
    width 100%
    height auto
    padding 20px 0
    box-sizing border-box
    background-color black
    .swiper-wrapper
      .swiper-slide-active
        .related-posts__post
          border-left none
    .swiper-box
      width 100%
      height 100%
      margin 0 auto
    .swiper-button-prev
      background-image url(../assets/swiper-prev-white.png)
      background-size contain
      opacity .75
      width 25px
      height 50px
      left 0
    .swiper-button-next
      background-image url(../assets/swiper-next-white.png)
      background-size contain
      opacity .75
      width 25px
      height 50px
      right 0
    .swiper-item
      height 100%
      text-align center
      font-size 18px 
      background black
      /* Center slide text vertically */
      display flex
      justify-content center
      align-items center
      .related-posts
        &__post
          box-sizing border-box
          width 100%
          padding 0 20px
          height 100%
          text-decoration none
          color hsla(0,0%,100%,.8)
          font-family -apple-system,Microsoft JhengHei,Segoe UI,Roboto,Ubuntu,Droid Sans,Helvetica Neue,sans-serif
          font-weight normal
          -webkit-font-smoothing auto
          border-left 1px solid hsla(0,0%,100%,.49)
          // font-size 17.6px
          // background-color black
        &__leading-img
          width 100%
        &__content-container
          // background-color black
        &__title
          text-align left
        &__description
          text-align justify
        &__divider
          width 1px
          height 340px
          background-color hsla(0,0%,100%,.49)

@media (max-width: 1100px)
  .related-posts-container
    width 100%
    margin 10% 0
    margin-bottom 0
    // .related-posts
    //   .swiper-item
    //     .related-posts__post
    //       border-left none

@media (max-width: 767px)
  .related-posts-container
    .related-posts
      .swiper-item
        .related-posts__post
          border-left none
</style>
