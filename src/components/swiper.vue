<template>
    <div class="box">
        <swiper class="swiper"
            :autoplay="true"
            :circular="true"
            easing-function="easeInOutCubic"
            @change="eventhandle('switchItem',$event)"
            current=0
            :indicator-dots="true"
        >
            <block v-for="(item, index) in list" :key="index">
                <swiper-item class="item">
                    <image v-show="index===currentIndex" :animation="animationData" mode="aspectFill" lazy-load="false" :src="item" class="slide-image"/>
                </swiper-item>
            </block>
        </swiper>
        <image v-if="isGif" class="img" src="../../static/images/save_the_date_pu.gif"/>
        <view class="container">
          <view :animation="animationData" class="view" :style="{'text-align': text[currentIndex].style, color: text[currentIndex].color}">{{text[currentIndex].title}}</view>
        </view>
    </div>
</template>

<script>
export default {
  name: 'GoodSwiper',
  props: ['list', 'isGif'],
  data () {
    return {
      currentIndex: 0,
      text: [
        {
          title: '2012.09.11 —— 2019.09.11',
          style: 'center',
          color: '#333'
        },
        {
          title: '我们在一起的时候不长, 不过2556天，七年',
          style: 'center',
          color: '#fff'
        },
        {
          title: '我和你就像两个小孩子，围着一个果酱罐，一点点的品尝，看看它究竟有多甜',
          style: 'center',
          color: '#fff'
        },
        {
          title: '你就是我最想遇见的那个特别的人，只要你在身边，不说话就十分美好',
          style: 'center',
          color: '#333'
        },
        {
          title: '再等等，等我们走过这匆匆岁月',
          style: 'center',
          color: '#333'
        },
        {
          title: '熬过人间疾苦',
          style: 'right',
          color: '#333'
        },
        {
          title: '黄昏下，我醉在你的怀里',
          style: 'right',
          color: '#fff'
        },
        {
          title: '做你最可爱的小老太太',
          style: 'left',
          color: '#333'
        },
        {
          title: '余生深爱',
          style: 'center',
          color: '#fff'
        }
      ],
      animationData: {},
      prevNum: -1,
      time: null
    }
  },
  methods: {
    random () { // 0-9随机数
      let num = Math.floor(Math.random() * 11)
      if (num === this.prevNum && num !== 10) {
        num++
      } else if (num === this.prevNum && num === 10) {
        num--
      }
      return num
    },
    eventhandle (prompt, res) {
      clearTimeout(this.time)
      this.time = null
      this.currentIndex = res.mp.detail.current
      let random = this.random()
      console.log(random)
      this['animate' + random]()
      // this['animate' + this.currentIndex]()
      // this.setAnimationData(this.currentIndex % 2)
      // this['animation' + this.currentIndex % 2]()
      this.animationData = {}
    },
    setAnimationData (index) {
      this['animation' + index]()
    },
    animate () {
      // rotateIn
      this.animation = wx.createAnimation({
        // 动画持续时间，单位ms，默认值 400
        duration: 800,
        timingFunction: 'ease',
        delay: 100,
        transformOrigin: '50% 50% 0'
      })
    },
    animate0 () {
      this.time = setTimeout(() => {
        this.animation.scale(0.2, 0.2)
        this.animation.rotate3d(0, 0, 1, 180).step()
        this.animation.scale(1, 1)
        this.animation.rotate3d(0, 0, 0, 0).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate1 () {
      this.time = setTimeout(() => {
        this.animation.rotate3d(0, 1, 0, 180).step()
        this.animation.rotate3d(1, 0, 0, 0).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate2 () {
      this.time = setTimeout(() => {
        this.animation.rotateY(0)
        this.animation.scale(0.5, 0.5).step()
        this.animation.rotateY(360)
        this.animation.scale(1, 1).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate3 () {
      this.time = setTimeout(() => {
        this.animation.skew(0, 60)
        this.animation.rotate3d(0, 1, 0, 0).step()
        this.animation.skew(0, 0)
        this.animation.rotate3d(0, 1, 0, 360).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate4 () {
      this.time = setTimeout(() => {
        this.time = setTimeout(() => {
          this.animation.rotate3d(1, 0, 0, -160).translate3d(-100, -100, 100).opacity(0).step()
          this.animation.rotate3d(1, 0, 0, 0).translate3d(0, 0, 0).opacity(1).step()
          this.animationData = this.animation.export()
        }, 100)
      }, 100)
    },
    animate5 () {
      this.time = setTimeout(() => {
        this.animation.rotateZ(0)
        this.animation.scale(0.5, 0.5).step()
        this.animation.rotateZ(360)
        this.animation.scale(1, 1).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate6 () {
      this.time = setTimeout(() => {
        this.animation.rotate3d(0, 0, 1, -180).translate3d(-100, -100, -200).skew(-0, 30).opacity(0).step()
        this.animation.rotate3d(0, 0, 1, 0).translate3d(0, 0, 0).skew(0, 0).opacity(1).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate7 () {
      this.time = setTimeout(() => {
        this.animation.skew(30, 30).step()
        this.animation.skew(0, 0).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate8 () {
      this.time = setTimeout(() => {
        this.animation.rotate3d(0, 1, 0, -180).translate3d(-100, -100, 200).skew(-30, 30).opacity(0).step()
        this.animation.rotate3d(1, 0, 1, 0).translate3d(0, 0, 0).skew(0, 0).opacity(1).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate9 () {
      this.time = setTimeout(() => {
        this.animation.rotate3d(0, 1, 0, 180).skew(30, 0).step()
        this.animation.rotate3d(0, 1, 0, 0).skew(0, 0).step()
        this.animationData = this.animation.export()
      }, 100)
    },
    animate10 () {
      this.time = setTimeout(() => {
        this.animation.rotate3d(0, 0, 1, 160).translate3d(100, 100, 100).opacity(0).step()
        this.animation.rotate3d(0, 0, 1, 0).translate3d(0, 0, 0).opacity(1).step()
        this.animationData = this.animation.export()
      }, 100)
    }
  },
  watch: {
    isGif (newValue, oldValue) {
      const that = this
      if (newValue) {
        that.isGif = newValue
      }
    }
  },
  onReady: function () {
    this.animate()
    // this.animate0()
    // this.animate1()
  }
}
</script>

<style lang="stylus" scoped>
.box
    position relative
    height 100%
    .swiper
        height 100%
        width 100%
        .item
            width 100%
            height 100%
            image
                width 100%
                height 100%
                display block
    .img
        position absolute;
        width 50%
        height 25%
        bottom 50rpx
        left 50%
        transform translateX(-50%)
        z-index 99
    .container
        position absolute
        width 100%
        top 0
        padding 50rpx
        box-sizing border-box
        z-index 100
        font-family "KaiTi", "YouYuan", "宋体", "SimSun"
</style>
