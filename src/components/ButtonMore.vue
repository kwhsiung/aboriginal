<template>
  <button :class="['hamburger', 'hamburger--spin', theme, state]" type="button" @click="emitEvent(theme, state)">
    <span class="hamburger-box">
      <span class="hamburger-inner">
        <span class="hamburger-up"></span>
        <span class="hamburger-down"></span>
      </span>
    </span>
  </button>
</template>
<script>
export default {
  props: {
    theme: {
      default: function () {
        return 'hamburger--background-light'
      }
    },
    state: {
      default: function () {
        return ''
      }
    }
  },
  methods: {
    emitEvent (theme, state) {
      if (state === 'closebtn') {
        return this.$emit('closeissues')
      }
      if (theme === 'hamburger--background-light') {
        return this.$emit('more')
      }
      if (theme === 'hamburger--background-dark') {
        return this.$emit('readissues')
      }
    }
  }
}
</script>

<style>
/*!
 * Hamburgers
 * @description Tasty CSS-animated hamburgers
 * @author Jonathan Suh @jonsuh
 * @site https://jonsuh.com/hamburgers
 * @link https://github.com/jonsuh/hamburgers
 */
.hamburger {
  /* background-color: #142d64; */
  display: flex;
  justify-content: center;
  align-items: center;
  width: 60px;
  height: 60px;
  border-radius: 60px;
  padding: 15px 15px;
  cursor: pointer;
  transition: opacity 0.15s linear, filter 0.15s linear, transform 0.40s ease-out;
  font: inherit;
  color: inherit;
  text-transform: none;
  /*background-color: transparent;*/
  border: 0;
  margin: 0;
  overflow: visible; 
  opacity: 0;
  animation-name: fadeIn;
  animation-duration: .7s;
  animation-delay: .7s;
  animation-fill-mode: forwards; }
.hamburger.hamburger--background-dark {
  background-color: white;
  box-shadow: 0px 0px 1px 1px #dcdcdc;
}
.hamburger.hamburger--background-light {
  background-color: #142d64;
}
  /* .hamburger--visible {
    animation-duration: .7s;
    animation-delay: 1s;
  } */
  .hamburger--invisible, .hamburger--background-dark--invisible, .hamburger--background-light--invisible {
    /* transition: opacity .1s cubic-bezier(0,0,.2,1);
    opacity: 0; */
    animation-name: fadeOut;
    animation-duration: .1s;
    animation-delay: 0s;
    animation-fill-mode: forwards;
  }
  
  .hamburger:hover {
    filter: brightness(80%);}
  .hamburger:focus {
    outline: 0;
  }

.hamburger-box {
  width: 30px;
  height: 24px;
  display: inline-block;
  position: relative; }

.hamburger-inner {
  display: block;
  top: 50%;
  margin-top: -2px; }
  .hamburger-inner, .hamburger-inner::before, .hamburger-inner::after, .hamburger-up, .hamburger-down {
    width: 30px;
    height: 4px;
    background-color: #fff;
    border-radius: 4px;
    position: absolute;
    transition-property: transform;
    transition-duration: 0.15s;
    transition-timing-function: ease; }
  .hamburger--background-dark .hamburger-inner, .hamburger--background-dark .hamburger-inner::before, .hamburger--background-dark .hamburger-inner::after, .hamburger--background-dark .hamburger-up, .hamburger--background-dark .hamburger-down {
    background-color: #142d64;
  }
  .hamburger--background-light .hamburger-inner, .hamburger--background-light .hamburger-inner::before, .hamburger--background-light .hamburger-inner::after, .hamburger--background-light .hamburger-up, .hamburger--background-light .hamburger-down {
    background-color: white;
  }
  .hamburger-inner::before {
    width: 30px;
    content: "";
    display: block; }
  .hamburger-inner::after {
    width: 30px;
    content: "";
    display: block; }
  .hamburger-inner::before {
    top: -15px; }
  .hamburger-inner::after {
    bottom: -15px; }
  .hamburger-up {
    /*display: block;*/
    width: 20px;
    left: 0px;
    top: -7px; }
  .hamburger-down {
    /*display: block;*/
    width: 20px;
    left: 0px;
    bottom: -7px; }

/*
   * Spin
   */
.hamburger--spin .hamburger-inner {
  transition-duration: 0.22s;
  transition-timing-function: cubic-bezier(0.55, 0.055, 0.675, 0.19); }
  .hamburger--spin .hamburger-inner::before {
    transition: top 0.1s 0.25s ease-in, opacity 0.1s ease-in; }
  .hamburger--spin .hamburger-inner::after {
    transition: bottom 0.1s 0.25s ease-in, transform 0.22s cubic-bezier(0.55, 0.055, 0.675, 0.19); }

.hamburger--spin.is-active {
  transform: translateX(calc(50vw - 50px - 30px));
  z-index: 1000;
}
.hamburger--spin.closebtn {
  z-index: 1000;
}
.hamburger--spin.is-active .hamburger-inner, .hamburger--spin.closebtn .hamburger-inner {
  transform: rotate(225deg);
  transition-delay: 0.12s;
  transition-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1); }
  .hamburger--spin.is-active .hamburger-inner::before, .hamburger--spin.closebtn .hamburger-inner::before {
    top: 0;
    opacity: 0;
    transition: top 0.1s ease-out, opacity 0.1s 0.12s ease-out; }
  .hamburger--spin.is-active .hamburger-up, .hamburger--spin.closebtn .hamburger-up {
    top: 0;
    opacity: 0;
    transition: top 0.1s ease-out, opacity 0.1s 0.12s ease-out; }
  .hamburger--spin.is-active .hamburger-inner::after, .hamburger--spin.closebtn .hamburger-inner::after {
    bottom: 0;
    transform: rotate(-90deg);
    transition: bottom 0.1s ease-out, transform 0.22s 0.12s cubic-bezier(0.215, 0.61, 0.355, 1); }
  .hamburger--spin.is-active .hamburger-down, .hamburger--spin.closebtn .hamburger-down {
    top: 0;
    opacity: 0;
    transition: top 0.1s ease-out, opacity 0.1s 0.12s ease-out; }
</style>

