<template>
    <div class="index">
        <div class="bg-swiper">
            <index-swiper :list="list"></index-swiper>
        </div>
        <image class="inv" src="../../static/images/inv.png"/>
        <div class="bg_music" v-if="isPlay" @tap="audioPlay">
            <image src="../../static/images/music_icon.png" class="musicImg music_icon"/>
            <image src="../../static/images/music_play.png" class="music_play pauseImg"/>
        </div>
        <div class="bg_music" v-else @tap="audioPlay">
            <image src="../../static/images/music_icon.png" class="musicImg"/>
            <image src="../../static/images/music_play.png" class="music_play playImg"/>
        </div>
        <div class="info" :animation="animationData">
            <div class="content">
                <h1>张海峰 & 周梦男</h1>
                <p>谨定于 2019年9月11日 （星期三）</p>
                <p>农历 八月十三 —— 举办婚礼</p>
                <p>席设：开原市上肥乡挠贝村</p>
                <br>
                <p>谨定于 2019年9月14日 （星期六）</p>
                <p>农历 八月十六 —— 回请</p>
                <p>席设：大连市旅顺开世温泉酒店</p>
                <image src="../../static/images/we.png" class="img_footer"/>
            </div>
        </div>
    </div>
</template>

<script>
import IndexSwiper from 'components/indexSwiper'
import tools from 'common/js/h_tools'
import cloud from '@/service/cloud'
const audioCtx = wx.createInnerAudioContext()
const event = require('../../utils/event.js')
const {zhf} = require('../../utils/index.js')
export default {
  name: 'Index',
  components: {
    IndexSwiper
  },
  data () {
    return {
      isPlay: true,
      list: [],
      flag: false
    }
  },
  onLoad () {
    const that = this
    if (zhf()) {
      this.flag = true
      that.getList()
    } else {
      this.list = [
        '../../static/material/1.jpg',
        '../../static/material/2.jpg'
      ]
    }
    event.on('musicPuase', this, function () {
      that.musicPause()
    })
  },
  onUnLoad () {
    event.remove('musicPuase', this)
  },
  onShow () {
    const that = this
    that.isPlay = true
    // that.getMusicUrl()
  },

  methods: {
    audioPlay () {
      const that = this
      if (that.isPlay) {
        audioCtx.pause()
        that.isPlay = false
        tools.showToast('您已暂停音乐播放~')
      } else {
        audioCtx.play()
        that.isPlay = true
        tools.showToast('背景音乐已开启~')
      }
    },
    musicPause () {
      const that = this
      if (that.isPlay) {
        audioCtx.pause()
        that.isPlay = false
        tools.showToast('您已暂停音乐播放~')
      }
    },
    getList () {
      const that = this
      wx.showNavigationBarLoading()
      cloud.get('weddingInvite').then((res) => {
        if (res.errMsg === 'collection.get:ok') {
          that.list = res.data[0].banner
          let musicUrl = res.data[0].music
          audioCtx.src = musicUrl
          audioCtx.loop = true
          audioCtx.autoplay = true
          audioCtx.play()
          wx.hideNavigationBarLoading()
        }
      })
    }
  },

  onShareAppMessage: function (res) {
    return {
      path: '/pages/index/main'
    }
  }
}
</script>

<style scoped lang="stylus">
@-webkit-keyframes musicRotate
  from
    -webkit-transformb rotate(0deg)
  to
    -webkit-transform rotate(360deg)
@-webkit-keyframes musicStop
  from
    -webkit-transform rotate(20deg)
  to
    -webkit-transform rotate(0deg)
@-webkit-keyframes musicStart
  from
    -webkit-transform rotate(0deg)
  to
    -webkit-transform rotate(20deg)
@-webkit-keyframes infoAnimation
  0%
    -webkit-transform scale(1) translate(0, 0)
  50%
    -webkit-transform scale(.9) translate(5px, 5px)
  100%
    -webkit-transform scale(1) translate(0, 0)
.index
  height 100%
  position relative
  .img
    width 100%
    height 100%
  .bg-swiper
    width 100%
    height 100%
  .inv
    position absolute
    top 20rpx
    left 89rpx
    width 572rpx
    height 69rpx
    z-index 9
  .bg_music
    position fixed
    right 0
    top 20rpx
    width 85rpx
    z-index 99
    display flex
    justify-content flex-start
    align-items flex-start
    .musicImg
      width 60rpx
      height 60rpx
    .music_icon
      animation musicRotate 3s linear infinite
    .music_play
      width 28rpx
      height 60rpx
      margin-left -10rpx
      transform-origin top
      -webkit-transform rotate(20deg)
    .playImg
      animation musicStop 1s linear forwards
    .pauseImg
      animation musicStart 1s linear forwards
  .info
    width 630rpx
    background rgba(255, 255, 255, 0.5)
    z-index 9
    position fixed
    bottom 40rpx
    left 50rpx
    padding 10rpx
    animation infoAnimation 12s linear infinite
    .content
      width 626rpx
      border 1rpx solid #000
      display flex
      flex-direction column
      justify-content flex-start
      align-items center
      position relative
      padding-bottom 30rpx
      h1
        font-size 50rpx
        height 100rpx
        line-height 100rpx
      p
        font-size 24rpx
        height 60rpx
        line-height 60rpx
      .img_footer
        position absolute
        bottom 0
        left 53rpx
        z-index 99
        height 14rpx
        width 520rpx
</style>
