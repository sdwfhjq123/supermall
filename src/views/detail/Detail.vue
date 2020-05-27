<template>
  <div id="detail">
    <detail-components />
    <detail-swiper :topImages="topImages" />
  </div>
</template>

<script>
import DetailComponents from './childComponents/DetailComponent'
import DetailSwiper from './childComponents/DetailSwiper'

import { getDetail } from 'network/detail.js'

export default {
  name: 'Detail',
  data () {
    return {
      id: null,
      topImages: []
    }
  },
  components: {
    DetailComponents,
    DetailSwiper
  },
  created () {
    this.id = this.$route.params.id

    getDetail(this.id).then(res => {
      this.topImages = res.result.itemInfo.topImages
    })
  }
}
</script>

<style>
</style>