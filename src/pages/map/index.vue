<template>
    <div class="map">
        <image mode="aspectFit" class="head-img" src="../../static/images/t1.png"/>
        <map class="content" id="map" :longitude="longitude" :latitude="latitude" show-location="true" :markers="markers" :scale="scale" @tap="toNav"></map>
        <div class="call">
            <div class="left" @tap="linkHe">
                <image src="../../static/images/he.png"/>
                <span>呼叫新郎</span>
            </div>
            <div class="right" @tap="linkShe">
                <image src="../../static/images/she.png"/>
                <span>呼叫新娘</span>
            </div>
        </div>
        <image class="footer" src="../../static/images/grren-flower-line.png"/>
    </div>
</template>

<script>
// import QQMap from 'common/js/qqmap-wx-jssdk.js'
export default {
  name: 'Map',
  data () {
    return {
      // qqSdk: '',
      latitude: 42.388641,
      longitude: 124.465431,
      scale: 11,
      markers: [{
        iconPath: '../../static/images/nav.png',
        id: 0,
        latitude: 42.388641,
        longitude: 124.465431,
        width: 50,
        height: 50,
        scale: 11
      }]
    }
  },
  onLoad () {
    const that = this
    const tomodifyDate = '2019-09-12'
    if (new Date().getTime() - new Date(tomodifyDate).getTime() >= 0) {
      that.latitude = 38.9131220000
      that.longitude = 121.1899880000
      that.markers = [{
        iconPath: '../../static/images/nav.png',
        id: 1,
        latitude: 38.9131220000,
        longitude: 121.1899880000,
        width: 50,
        height: 50,
        scale: 16
      }]
    }
  },
  methods: {
    toNav () {
      let that = this
      wx.openLocation({
        latitude: that.latitude,
        longitude: that.longitude,
        scale: that.scale
      })
    },

    linkHe () {
      wx.makePhoneCall({
        phoneNumber: '13478979454'
      })
    },

    linkShe () {
      wx.makePhoneCall({
        phoneNumber: '13644236815'
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
.map
    height 100%
    background #fff
    .head-img
      width 100%
      height 180rpx
      margin-bottom 50rpx
    .content
      width 690rpx
      margin-left 30rpx
      height 600rpx
      margin-bottom 30rpx
      border-radius 16rpx
    .call
      display flex
      justify-content space-around
      align-items center
      width 100%
      margin-bottom 20rpx
      .left, .right
        flex 1
        display flex
        flex-direction column
        justify-content center
        align-items center
        image
          height 64rpx
          width 64rpx
        span
          height 50rpx
          line-height 50rpx
          font-size 24rpx
          color #6B4F4E
    .footer
      height 80rpx
      width 716rpx
      margin-left 17rpx
</style>
