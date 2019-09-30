<template>
    <div class="section">
        <video id="myVideo" 
          src="cloud://miniprogram-ar6ho.6d69-miniprogram-ar6ho/张海峰&周梦男~6.mp4" 
          poster="cloud://miniprogram-ar6ho.6d69-miniprogram-ar6ho/movie.jpg"
          :danmu-list="danmuList"
        >
				</video>
        <div class="btn-area">
            <input @blur="bindInputBlur" placeholder="请输入临时弹幕" v-model="inputValue" placeholder-style="color:#bbb"/>
            <button @tap="bindSendDanmu">点击生成临时弹幕</button>
            <image src="../../static/images/close1.png" @tap="close"/>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Video',
  data () {
    return {
      inputValue: '',
      danmuList: [
        {
          text: '测试弹幕1',
          color: 'red',
          time: 5
        },
        {
          text: '测试弹幕2',
          color: 'blue',
          time: 8
        },
        {
          text: '测试弹幕3',
          color: 'green',
          time: 10
        }
      ]
    }
  },

  onLoad () {
    const that = this
    that.getMessageList()
    that.videoContext = wx.createVideoContext('myVideo')
  },

  methods: {
    bindInputBlur (e) {
      // let that = this
      // that.inputValue = e.mp.detail.value
    },

    bindSendDanmu: function () {
      let that = this
      that.videoContext.sendDanmu({
        text: that.inputValue,
        color: that.getRandomColor()
      })
      that.inputValue = ''
    },

    getRandomColor () {
      let rgb = []
      for (let i = 0; i < 3; ++i) {
        let color = Math.floor(Math.random() * 256).toString(16)
        color = color.length === 1 ? '0' + color : color
        rgb.push(color)
      }
      return '#' + rgb.join('')
    },

    getMessageList () {
      const that = this
      const db = wx.cloud.database()
      const message = db.collection('msg')
      message.get().then(res => {
        let data = res.data.reverse()
        let arr = []
        if (data.length > 0) {
          data.forEach((item, index) => {
            console.log(item)
            arr.push({
              text: item.userMsg,
              color: that.getRandomColor(),
              time: 1 + (index * 2)
            })
          })
          that.danmuList = arr
          console.log(that.danmuList)
        }
      })
    },

    close () {
      const that = this
      that.$emit('closeVideo')
    }
  }
}
</script>

<style lang="stylus" scoped>
.section
    width 750rpx
    position relative
    #myVideo
        height 422rpx
        width 100%
        margin-bottom 20rpx
    .btn-area
        input
            width 660rpx
            border 1rpx solid #e5e5e5
            padding-left 30rpx
            height 80rpx
            line-height 80rpx
            font-size 30rpx
            color #ccc
            margin-bottom 30rpx
            margin-left 30rpx
            border-radius 16rpx
        button
            height 92rpx
            width 690rpx
            border-radius 16rpx
            background #1AAD19
            color #fff
            line-height 92rpx
            text-align center
            font-size 36rpx
            margin-bottom 40rpx
        image
            width 80rpx
            height 80rpx
            background #fff
            margin 0 auto
</style>
