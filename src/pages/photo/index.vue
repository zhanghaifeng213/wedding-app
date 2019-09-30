<template>
    <div class="photo">
        <h-swiper :list="list" :isGif="isGif"></h-swiper>
    </div>
</template>

<script>
import HSwiper from 'components/swiper'
import cloud from '@/service/cloud'
const {zhf} = require('../../utils/index.js')
// import imgData from 'common/json/imgData.json'
export default {
  name: 'Photo',
  components: {
    HSwiper
  },
  onLoad () {
    const that = this
    that.isGif = !that.isGif
    if (zhf()) {
      this.flag = true
      that.getList()
    } else {
      this.list = [
        '../../static/material/1.jpg',
        '../../static/material/2.jpg'
      ]
    }
  },

  data () {
    return {
      list: [],
      isGif: false,
      flag: false
    }
  },
  methods: {
    getList () {
      const that = this
      wx.showNavigationBarLoading()
      cloud.get('weddingInvite').then((res) => {
        if (res.errMsg === 'collection.get:ok') {
          that.list = res.data[0].banner
          wx.hideNavigationBarLoading()
        }
      })
    },
    eventhandle (event) {
      console.log(event.detail.current)
    }
  }
}
</script>

<style lang="stylus" scoped>
.photo
    height 100%
</style>

