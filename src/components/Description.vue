<template>
  <div>
    <slot name="title"></slot>

    <!--Container for animation-->
    <div class="section__subtitle-container--invisible section__subtitle-container">
      <slot name="subtitle"></slot>
    </div>

    <slot name="issues"></slot>
    <slot name="credits"></slot> 
    <slot name="related-posts"></slot>
    <slot name="projects"></slot> 
    <slot name="fbcomments"></slot>
  </div>
</template>
<style lang="stylus">
.section__description
  .section
    &__subtitle
      color white
      // font-weight 500
      font-size 24px
      line-height 44px
      // margin 1.5em 0
      text-shadow 2px 2px 8px black
      &.marginless
        margin 0
      &.margintop-less
        margin-top 0
      &.marginbottom-less
        margin-bottom 0
      &.list-margin-normal
        margin 1em 0
      .mark--blue
        color white
        background-color #142d64

    &--focus
      color yellow
      &.large
        font-size 46px
        font-family Georgia
        font-weight bold
        line-height 46px
        width 80px
        height 50px
        display inline-block
        text-align center
    &--gray
      color gray !important
      text-shadow none !important
    &--red
      color #bf2929 !important
      text-shadow none !important
      display flex
      flex-direction column
      justify-content flex-start
      align-items center
    &--small
      font-size 20px
      line-height initial
      
  &--left
    .section
      &__title
        color white
        font-size 46px
        line-height 1.25
        font-weight 500
        margin 0
        
  &--left-wide
    .section
      &__subtitle
        color white
        font-size 46px
        line-height 1.25
        font-weight 500
        margin 0
        &.section--focus
          color yellow
          font-family Georgia
          font-weight bold
      &__title
        color white
        font-size 24px
        line-height 44px

  &--center, &--center-wide
    .section
      &__title
        color white
        font-size 60px
        // line-height 1.25
        font-weight 400
        margin 0
    .section
      &__subtitle
        text-shadow none !important
  &--space-around
    .section
      &__title
        color white
        font-size 46px
        // line-height 1.25
        font-weight 500
        // margin 0
      &__subtitle
        text-shadow none !important
      &__issue-title
        color white
        font-size 36px
        // line-height 1.25
        font-weight 500
        margin 0
.desktop-hide
  display none

.section
  &__title
    // &--active
    opacity 0
    animation-name fadeInUp
    animation-duration .7s
    animation-delay 0s
    animation-fill-mode forwards
    &--invisible
      animation-name fadeOut
      animation-duration .1s
      animation-delay 0s
      animation-fill-mode forwards


  &__subtitle-container
    // &--active
    opacity 0
    animation-name fadeInUp
    animation-duration .7s
    animation-delay .7s
    animation-fill-mode forwards
    &--invisible
      animation-name fadeOut
      animation-duration .1s
      animation-delay 0s
      animation-fill-mode forwards

    .self-animate
      opacity 0
      animation-name fadeInUp
      animation-duration .7s
      animation-fill-mode forwards
      &--invisible
        animation-name fadeOut
        animation-duration .1s
        animation-delay 0s
        animation-fill-mode forwards

    .activity
      opacity 0
      animation-name fadeIn
      animation-duration .5s
      // animation-delay .8s * $i
      animation-fill-mode forwards
      &--invisible
        animation-name fadeOut
        animation-duration .1s
        animation-delay 0s
        animation-fill-mode forwards

    .x
      z-index 100
      &--invisible
        animation-name fadeOut !important
        animation-duration .1s !important
        animation-fill-mode forwards !important
    #x1
      content ''
      background-image url('../assets/x.png')
      background-size contain
      background-position center center
      background-repeat no-repeat
      display inline-block
      width 62px
      height 83px
      align-self flex-start
      position absolute
      top 0px
      left -60px
      opacity 0
      animation-name fadeIn
      animation-duration .7s
      animation-delay 1.4s
      animation-fill-mode forwards
    #x2
      content ''
      background-image url('../assets/x.png')
      background-size contain
      background-position center center
      background-repeat no-repeat
      display inline-block
      width 62px
      height 83px
      align-self flex-start
      position absolute
      top 0px
      left 20px
      opacity 0
      animation-name fadeIn
      animation-duration .7s
      animation-delay 3.5s
      animation-fill-mode forwards

  .li--inside
    padding 0
    list-style-position inside
    display flex
    flex-direction column
    justify-content center
    align-items center
    position relative
    .marginbottom
      margin-bottom 10%
    li
      width calc(100% + 20px)

  .inactive
    transition opacity .7s cubic-bezier(0,0,.2,1)
    opacity 0

  .projects__container
    box-sizing border-box
    width 80%
    height 450px
    margin 5% 0
    // padding 10px
    // border 1px solid white
    display flex
    flex-direction column
    justify-content center
    align-items center
    &-title
      color white
      font-size 24px
      margin 0
      margin-bottom 24px
  .fb-comments__container
    width 80%
      

@media (max-width: 1600px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.3)
        line-height calc(44px/1.3)
      &--focus
        &.large
          font-size calc(46px/1.3)
          line-height calc(46px/1.3)
      &--small
        font-size calc(20px/1.3)
      &__subtitle-container
        #x1
          width calc(62px/1.3)
          height calc(83px/1.3)
          // top calc(-25px/1.3)
          left calc(-60px/1.3)
        #x2
          width calc(62px/1.3)
          height calc(83px/1.3)
          // top calc(-25px/1.3)
          left calc(20px/1.3)
    &--left
      .section
        &__title
          font-size calc(46px/1.3)
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/1.3)
          line-height 1.25
        &__title
          font-size calc(24px/1.3)
          line-height calc(44px/1.3)
    &--center, &--center-wide
      .section
        &__title
          font-size calc(60px/1.3)
    &--space-around
      .section
        &__title
          font-size calc(46px/1.3)
        &__issue-title
          font-size calc(36px/1.3)
    // .projects__container
      // height 70vh

@media (max-width: 1439px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.6)
        line-height calc(44px/1.6)
      &--focus
        &.large
          font-size calc(46px/1.6)
          line-height calc(46px/1.6)
      &--small
        font-size calc(20px/1.6)
      &__subtitle-container
        #x1
          width calc(62px/1.6)
          height calc(83px/1.6)
          // top calc(-25px/1.6)
          left calc(-60px/1.6)
        #x2
          width calc(62px/1.6)
          height calc(83px/1.6)
          // top calc(-25px/1.6)
          left calc(20px/1.6)
    &--left
      .section
        &__title
          font-size calc(46px/1.6)
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/1.6)
          line-height 1.25
        &__title
          font-size calc(24px/1.6)
          line-height calc(44px/1.6)
    &--center, &--center-wide
      .section
        &__title
          font-size calc(60px/1.6)
    &--space-around
      .section
        &__title
          font-size calc(46px/1.6)
        &__issue-title
          font-size calc(36px/1.6)

@media (max-width: 1279px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.7)
        line-height calc(44px/1.7)
      &--focus
        &.large
          font-size calc(46px/1.7)
          line-height calc(46px/1.7)
      &--small
        font-size calc(20px/1.7)
      &__subtitle-container
        #x1
          width calc(62px/1.7)
          height calc(83px/1.7)
          // top calc(-25px/1.7)
          left calc(-60px/1.7)
        #x2
          width calc(62px/1.7)
          height calc(83px/1.7)
          // top calc(-25px/1.7)
          left calc(20px/1.7)
    &--left
      .section
        &__title
          font-size calc(46px/1.7)
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/1.7)
          line-height 1.25
        &__title
          font-size calc(24px/1.7)
          line-height calc(44px/1.7)
    &--center, &--center-wide
      .section
        &__title
          font-size calc(60px/1.7)
    &--space-around
      .section
        &__title
          font-size calc(46px/1.7)
        &__issue-title
          font-size calc(36px/1.7)


// For IPad
@media (max-width: 1100px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.2)
        line-height calc(44px/1.2)
      &--focus
        &.large
          font-size calc(46px/1.2)
          line-height calc(46px/1.2)
      &--small
        font-size calc(20px/1.2)
        line-height 1.5
      &__subtitle-container
        #x1
          width calc(62px/1.2)
          height calc(83px/1.2)
          left calc(-60px/1.2)
        #x2
          width calc(62px/1.2)
          height calc(83px/1.2)
          left calc(-60px/1.2)
        .li--inside
          align-items flex-start
          margin-left 30px
        .li--outside
          padding 0
          list-style-position inside
        .mobile-marginless
          margin-left 2px
    &--left
      .section
        &__title
          font-size calc(46px/1.2)
          line-height 1.25
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/1.2)
          line-height 1.25
        &__title
          font-size calc(24px/1.2)
          line-height calc(44px/1.2)
    &--center, &--center-wide
      align-items flex-start
      .section
        &__title
          font-size calc(46px/1.2)
          line-height 1.25
    &--space-around
      .section
        &__title
          padding-left 15px
          font-size calc(46px/1.2)
        &__issue-title
          font-size calc(36px/1.2)
      .space-around
        flex-direction column
        align-items flex-start
        &__container
          display flex !important
          flex-direction column
          justify-content center
          align-items flex-start
          border-top 1px solid white
          margin 0 !important
        &>div
          width 100%
          height 22vh
          padding 10px 15px 0px 15px
        .section__subtitle
          width 100%
        .vertical-line
          display none
        .desktop-hide
          display inline-block
          vertical-align middle
          line-height calc(44px/1.2)
    .credits
      display flex
      flex-direction column
      justify-content center
      align-items flex-start
    .projects__container
      width 100%
      height 400px
      margin 15% 0
      // padding 40px 10px 0px 10px
      // border 1px solid white
      display flex
      flex-direction column
      justify-content center
      align-items center
      &-title
        color white
        font-size 24px
        margin 0
        margin-bottom 24px
    .fb-comments__container
      width 100%
  .mobile-hide
    display none
  .desktop-hide
    display initial

// For Iphone 6+
@media (max-width: 767px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.2)
        line-height calc(44px/1.6)
      &--focus
        &.large
          font-size calc(46px/1.6)
          line-height calc(46px/1.6)
          width 60px
      &--small
        font-size calc(20px/1.6)
        line-height 1.5
      &__subtitle-container
        #x1
          width calc(62px/1.6)
          height calc(83px/1.6)
          left calc(-60px/1.6)
        #x2
          width calc(62px/1.6)
          height calc(83px/1.6)
          left calc(-60px/1.6)
    &--left
      .section
        &__title
          font-size calc(46px/1.6)
          line-height 1.25
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/1.6)
        &__title
          font-size calc(24px/1.2)
          line-height calc(44px/1.7)
    &--center, &--center-wide
      .section
        &__title
          font-size calc(46px/1.6)
    &--space-around
      .section
        &__title
          font-size calc(46px/1.6)
        &__issue-title
          font-size calc(36px/1.6)
      .space-around
        .section__subtitle
          width 70%
        .desktop-hide
          line-height calc(44px/1.6)

    .projects__container
      width 100%
      height 400px
      margin 25% 0
      // padding 40px 10px 0px 10px
      // border 1px solid white
      display flex
      flex-direction column
      justify-content center
      align-items center
      &-title
        color white
        font-size 24px
        margin 0
        margin-bottom 24px

// For Iphone 6
@media (max-width: 413px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.4)
        line-height calc(44px/1.9)
      &--focus
        &.large
          font-size calc(46px/1.8)
          line-height calc(46px/1.8)
      &--small
        font-size calc(20px/1.8)
      &__subtitle-container
        #x1
          width calc(62px/1.8)
          height calc(83px/1.8)
          left calc(-60px/1.8)
        #x2
          width calc(62px/1.8)
          height calc(83px/1.8)
          left calc(-60px/1.8)
    &--left
      .section
        &__title
          font-size calc(46px/1.8)
          line-height 1.25
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/1.8)
          line-height 1.25
        &__title
          font-size calc(24px/1.4)
          line-height calc(44px/1.9)
    &--center, &--center-wide
      .section
        &__title
          font-size calc(46px/1.8)
          line-height 1.25
    &--space-around
      .section
        &__title
          font-size calc(46px/1.8)
        &__issue-title
          font-size calc(36px/1.8)
      .space-around
        .desktop-hide
          line-height calc(44px/1.8)
    
// For IPhone 5/5S/SE
@media (max-width: 374px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.7)
        line-height calc(44px/2.2)
      &--focus
        &.large
          font-size calc(46px/2.1)
          line-height calc(46px/2.1)
      &--small
        font-size calc(20px/2.1)
        line-height 1.5
      &__subtitle-container
        #x1
          width calc(62px/2.1)
          height calc(83px/2.1)
          left calc(-60px/2.1)
        #x2
          width calc(62px/2.1)
          height calc(83px/2.1)
          left calc(-60px/2.1)
    &--left
      .section
        &__title
          font-size calc(46px/2.1)
          line-height 1.25
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/2.1)
          line-height 1.25
        &__title
          font-size calc(24px/2.0)
          line-height calc(44px/2.5)
    &--center, &--center-wide
      .section
        &__title
          font-size calc(46px/2.1)
          line-height 1.25
    &--space-around
      .section
        &__title
          font-size calc(46px/2.1)
        &__issue-title
          font-size calc(36px/2.1)
      .space-around
        .desktop-hide
          line-height calc(44px/2.1)
</style>
