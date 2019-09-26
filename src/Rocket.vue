<template>
  <div class="rocket" v-if="!isMobile">
    <div @click="scrollToTop" v-if="show" :class="toTop ? 'spaceship launch' : 'spaceship'">
      <div class="spaceshipBody">
        <div class="spaceshipTop"></div>
        <div class="spaceshipWindows"> <span></span> <span></span> </div>
      </div>
      <div class="spaceshipBottom"> 
        <span></span> 
        <span></span> 
      </div>
      <div class="spaceshipBottomSide"></div>
      <div :class="toTop ? 'fire burn' : 'fire'"> <span></span> <span></span> <span></span>
        <div class="glow"></div>
      </div>
    </div>
    <div v-if="show" class="shadow"></div>
  </div>
</template>

<script>
import debounce from 'lodash.debounce'
import { isMobile } from './utils'

export default {
  props: {
    threshold: {
      type: Number,
      default: 300
    }
  },
  data () {
    return {
      scrollTop: null,
      toTop: false,
    }
  },
  mounted () {
    this.isMobile = isMobile()
    if(this.isMobile) return console.log('mobile does not load rocket')
    this.scrollTop = this.getScrollTop()
    window.addEventListener('scroll', debounce(() => {
      this.scrollTop = this.getScrollTop()
    }, 100))
  },
  methods: {
    getScrollTop () {
      return window.pageYOffset
        || document.documentElement.scrollTop
        || document.body.scrollTop || 0
    },
    scrollToTop () {
      if(this.toTop) return

      this.toTop = true

      setTimeout(() => {
        this.toTop = false
        window.scrollTo({ top: 0, behavior: 'smooth' })
        this.scrollTop = 0
      }, 800)
    }
  },
  computed: {
    show () {
      return this.scrollTop > this.threshold
    }
  }
}
</script>

<style lang='stylus' scoped>
.rocket 
  width: 150px;
  position: fixed;
  z-index: 1;
  bottom: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  .spaceship 
    width: 150px;
    height: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    transition: all 1s ease;
    bottom: -2rem;
    right: 0;
    animation: float 2s ease infinite alternate;
    &:hover 
      cursor: pointer;
    &.launch
      animation: launch 1s ease-in;
    .spaceshipBody 
      width: 70%;
      height: 80%;
      background-color: white;
      border-bottom-left-radius: 100%;
      border-bottom-right-radius: 100%;
      border-top-left-radius: 100%;
      border-top-right-radius: 100%;
      display: flex;
      justify-content: center;
      position: relative;
      z-index: 1;
      box-shadow: inset 0px -173px 0px -80px white, inset 0px -213px 0px -60px #e4e4e4;
      transform: scale(0.5, 0.5);
      &:before 
      &:after 
        content: "";
        position: absolute;
        transform: perspective(10em) rotateX(-50deg);
        border-bottom-left-radius: 50px;
        border-bottom-right-radius: 50px;
      &:before 
        width: calc(100% - 36px);
        height: 42%;
        background-color: inherit;
        bottom: -30px;
        box-shadow: inset 0px -33px 0px 0px rgba(0, 0, 0, 0.1);
      &:after 
        width: 45%;
        height: 40px;
        background-color: #f95959;
        bottom: -20px;
        z-index: -1;
      .spaceshipTop 
        width: 100%;
        height: 240px;
        border-radius: 100%;
        overflow: hidden;
        position: relative;
        box-shadow: inset -12px 17px 0px -7px rgba(0, 0, 0, 0.15);
        &:before 
          content: "";
          background-color: #4ba3b7;
          position: absolute;
          width: 100%;
          height: 100px;
          left: calc(50% - 54%);
          border-radius: 100%;
          top: -55px;
          border: 2px solid white;
          box-shadow: inset -18px 56px 0px 3px rgba(0, 0, 0, 0.1), 0px 0px 0px 6px #f95959;
      .spaceshipWindows 
        display: flex;
        justify-content: center;
        align-items: center;
        width: 300px;
        height: 300px;
        position: absolute;
        span 
          background-color: #ace7ef;
          box-shadow: inset -4px 4px 0px 0px rgba(0, 0, 0, 0.3), inset 0px 0px 0px 2px white;
          border: 4px solid #f95959;
          z-index: 2;
          position: absolute;
          border-radius: 100%;
          overflow: hidden;
          &:before 
            position: absolute;
            content: "";
            background-color: white;
            width: 200%;
            height: 100%;
            transform: rotate(45deg);
            opacity: 0.4;
          &:nth-child(1) 
            width: 30px;
            height: 30px;
            top: 23%;
            :before 
              top: 10px;
              right: 0px;
          &:nth-child(2) 
            width: 45px;
            height: 45px;
            top: 40%;
            :before 
              top: 12px;
              right: 0px;
    .spaceshipBottom 
      display: flex;
      justify-content: center;
      align-items: center;
      width: 300px;
      height: 300px;
      position: absolute;
      transform: scale(0.5, 0.5);
      span
        &:before 
          content: "";
          position: absolute;
          background-color: white;
          width: 2px;
          height: 120%;
          border-radius: 20px;
        &:nth-child(1)
        &:nth-child(2)
          width: 50px;
          height: 130px;
          bottom: 6%;
          transform: perspective(10em) rotateX(60deg) translateZ(-1px);
        &:nth-child(1) 
          left: 32%;
          :before 
            left: 0px;
            border-right: 2px solid #f95959;
        &:nth-child(2) 
          right: 32%;
          :before 
            right: 0px;
            border-left: 2px solid #f95959;
    .spaceshipBottom span
    .spaceshipBottomSide
      background-color: #4ba3b7;
      border-radius: 10px;
      position: absolute;
      overflow: hidden;
      box-shadow: inset -5px -3px 0px 0px rgba(0, 0, 0, 0.2);
    .spaceshipBottomSide 
      width: 7.5px;
      height: 40px;
      z-index: 2;
      bottom: 5rem;
      box-shadow: inset -2.5px -1.5px 0px 0px rgba(0, 0, 0, 0.18);
    .fire 
      display: flex;
      justify-content: center;
      align-items: center;
      width: 300px;
      height: 100px;
      position: absolute;
      bottom: 1rem;
      transform: scale(0.5, 0.5)
      &.burn span 
        border-radius: 50px;
        top: 0;
        position: absolute;
        background-color: #ffd460;
        height: inherit;
        animation: fire 0.8s ease infinite alternate;
        &:nth-child(1) 
          width: 6px;
          height: 40px;
          left: 44%;
          transform: translateY(27px);
          box-shadow: inset 0px -7px 10px #ea5455, inset 0px -19px 10px #ffc175, 0px -7px 10px #ea5455;
          animation-delay: 0.2s;
          &:after 
            position: absolute;
            content: "";
            width: 4px;
            height: 60%;
            border-radius: 50px;
            background-color: #ffd460;
            bottom: 0;
            transform: translate(8px, 15px);
            box-shadow: inset 0px -5px 10px #ea5455, inset 0px -19px 10px #ffc175, 0px -7px 10px #ea5455;
        &:nth-child(2) 
          width: 10px;
          height: 60px;
          left: calc(50% - 8px);
          transform: translateY(35px);
          box-shadow: inset 0px -10px 10px #ea5455, inset 0px -30px 10px #ffc175, 0px -7px 10px #ea5455;
          &:after 
            position: absolute;
            content: "";
            width: 10px;
            height: 100%;
            border-radius: 10px;
            background-color: #ffd460;
            top: 0;
            transform: translate(-6px, -25px);
            box-shadow: inset 0px -5px 10px #ea5455, inset 0px -15px 10px #ffc175, 0px -7px 10px #ea5455;
        &:nth-child(3) 
          width: 10px;
          height: 40px;
          right: 45%;
          transform: translateY(27px);
          box-shadow: inset 0px -5px 10px #ea5455, inset 0px -30px 10px #ffc175, 0px -7px 10px #ea5455;
          animation-delay: 0.4s;
          &:after 
            position: absolute;
            content: "";
            width: 6px;
            height: 180%;
            border-radius: 10px;
            background-color: #ffd460;
            top: 0;
            transform: translate(-6px, -15px);
            box-shadow: inset 0px -5px 10px #ea5455, inset 0px -20px 10px #ffc175, 0px -7px 10px #ea5455;
      &.burn .glow 
        position: absolute;
        width: 0px;
        height: 0px;
        border-radius: 100%;
        box-shadow: 0px 0px 50px 20px #ea5455;
        opacity: 1;
        animation: glow 0.8s ease infinite alternate;
  .shadow 
    width: 150px;
    height: 30px;
    background-color: black;
    position: absolute;
    border-radius: 100%;
    opacity: 0.2;
    bottom: 0;
    z-index: -1;
    animation: shadow 2s ease infinite alternate;
    transition: all 0.5s ease;
    transform: scale(0.5, 0.5)
@keyframes float 
  0% 
    transform: translateY(10px);
  100% 
    transform: translateY(0px);
@keyframes launch 
  0% 
    bottom: -2rem;
    transform: translatex(2px);
  10% 
    transform: translatex(-2px);
  20%
    transform: translatex(2px);
  30% 
    transform: translatex(-2px);
  60% 
    transform: translatex(0px);
    bottom: 150px;
  100% 
    bottom: 120vh
@keyframes fire 
  0% 
    height: 10px;
    bottom: 0;
    50% 
      top: 0;
    100% 
      height: 20px;
      bottom: 0;
@keyframes glow 
  0% 
    box-shadow: 0px 0px 50px 20px #ea5455;
  100% 
    box-shadow: 0px 0px 50px 25px #ea5455;
@keyframes shadow {
  0% {
    width: 150px;
  }
  100% {
    width: 120px;
  }
}
</style>
