<template>
  <div>
    <slot name="title"></slot>

    <!--Container for animation-->
    <div class="section__subtitle-container--invisible section__subtitle-container">
      <slot name="subtitle"></slot>
    </div>

    <slot name="issues"></slot>
  </div>
</template>
<style lang="stylus">
.section__description
  .section
    &__subtitle
      color white
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
        // line-height 1.25
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

  &--space-around
    .section
      &__title
        color white
        font-size 46px
        // line-height 1.25
        font-weight 500
        margin 0
      &__issue-title
        color white
        font-size 36px
        // line-height 1.25
        font-weight 500
        margin 0

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
      top -25px
      left -30px
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
      top -25px
      left 50px
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

// @media (max-width: 1680px)
//   // .section
//   //   &__title
//   //     font-size 34px
//   //   &__subtitle
//   //     font-size 18.4px
//   //   &--small
//   //     font-size 16.4px
//   .section__description
//     .section
//       &__subtitle
//         font-size calc(30px/1.1)
//         line-height calc(51px/1.1)
//       &--focus
//         &.large
//           font-size calc(55px/1.1)
//           line-height calc(56px/1.1)
//       &--small
//         font-size calc(25px/1.1)
//       &__subtitle-container
//         .x:before
//           width calc(62px/1.1)
//           height calc(83px/1.1)
//           margin-top calc(-110px/1.1)
//     &--left
//       .section
//         &__title
//           font-size calc(55px/1.1)
//     &--center, &--center-wide
//       .section
//         &__title
//           font-size calc(80px/1.1)

@media (max-width: 1440px)
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
          top calc(-25px/1.3)
          left calc(-30px/1.3)
        #x2
          width calc(62px/1.3)
          height calc(83px/1.3)
          top calc(-25px/1.3)
          left calc(50px/1.3)
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

@media (max-width: 1280px)
  .section__description
    .section
      &__subtitle
        font-size calc(24px/1.5)
        line-height calc(44px/1.5)
      &--focus
        &.large
          font-size calc(46px/1.5)
          line-height calc(46px/1.5)
      &--small
        font-size calc(20px/1.5)
      &__subtitle-container
        #x1
          width calc(62px/1.5)
          height calc(83px/1.5)
          top calc(-25px/1.5)
          left calc(-30px/1.5)
        #x2
          width calc(62px/1.5)
          height calc(83px/1.5)
          top calc(-25px/1.5)
          left calc(50px/1.7)
    &--left
      .section
        &__title
          font-size calc(46px/1.5)
    &--left-wide
      .section
        &__subtitle
          font-size calc(46px/1.5)
          line-height 1.25
        &__title
          font-size calc(24px/1.5)
          line-height calc(44px/1.5)
    &--center, &--center-wide
      .section
        &__title
          font-size calc(60px/1.5)
    &--space-around
      .section
        &__title
          font-size calc(46px/1.5)
        &__issue-title
          font-size calc(36px/1.5)

// @media (max-width: 1024px)
//   .section__description
//     .section
//       &__subtitle
//         font-size calc(24px/1.5)
//         line-height calc(44px/1.5)
//       &--focus
//         &.large
//           font-size calc(46px/1.5)
//           line-height calc(46px/1.5)
//       &--small
//         font-size calc(20px/1.5)
//       &__subtitle-container
//         .x:before
//           width calc(62px/1.5)
//           height calc(83px/1.5)
//           margin-top calc(-90px/1.5)
//     &--left
//       .section
//         &__title
//           font-size calc(46px/1.5)
//     &--center, &--center-wide
//       .section
//         &__title
//           font-size calc(60px/1.5)

@media (max-height: 600px)
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
        .x:before
          width calc(62px/1.7)
          height calc(83px/1.7)
          margin-top calc(-90px/1.7)
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
</style>
