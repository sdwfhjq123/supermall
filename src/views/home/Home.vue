<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-controller
      class="tab-controller"
      :titles="['新款', '流行', '精选']"
      @tabClick="tabClick"
      v-show="isTabFixed"
      ref="tabController1"
    />

    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      :pull-up-load="true"
      @onScroll="onScroll"
      @pullingUp="loadMore"
    >
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-controller :titles="['新款', '流行', '精选']" @tabClick="tabClick" ref="tabController2" />
      <goods-list :goods="showGoods" />
    </scroll>

    <back-top @click.native="backClick" v-show="showBackTop" />
  </div>
</template>

<script>
import NavBar from 'components/content/NavBar'
import HomeSwiper from './childComponents/HomeSwiper'
import RecommendView from './childComponents/RecommendView'

import FeatureView from './childComponents/FeatureView'
import TabController from 'components/content/tabController/TabController'
import GoodsList from 'components/content/goods/GoodsList'
import Scroll from 'components/common/Scroll'
import BackTop from 'components/content/backTop/BackTop'

import { getHomeMultiData, getHomeGoods } from "network/home"

export default {
  name: "Home",
  components: {
    NavBar,
    HomeSwiper,
    RecommendView,

    FeatureView,
    TabController,
    GoodsList,
    Scroll,
    BackTop
  },
  data () {
    return {
      banners: [],
      recommends: [],
      goods: {
        'pop': { page: 0, list: [] },
        'new': { page: 0, list: [] },
        'sell': { page: 0, list: [] },
      },
      currentType: 'new',
      isShowBackTop: false,
      tabOffsetTop: 0,
      isTabFixed: false
    }
  },
  created () {
    this.getHomeMultiData()
    this.getHomeGoods('new')
    this.getHomeGoods('pop')
    this.getHomeGoods('sell')
  },
  mounted () {

    const refresh = this.debounce(this.$refs.scroll.refresh)
    this.$bus.$on('itemImageLoadCompleted', () => {
      refresh()
    })
  },
  computed: {
    showGoods () {
      return this.goods[this.currentType].list
    },
    showBackTop () {
      return this.isShowBackTop
    }
  },

  methods: {
    /**
     * 事件监听
     */
    debounce (fun, delay = 300) {
      let timer = null
      return function (...args) {
        if (timer) clearTimeout(timer)
        timer = setTimeout(() => {
          fun.apply(this, args)
        }, delay)
      }
    },
    tabClick (index) {
      switch (index) {
        case 0:
          this.currentType = 'new'
          break
        case 1:
          this.currentType = 'pop'
          break
        case 2:
          this.currentType = 'sell'
          break
      }
      this.$refs.tabController1.curIndex = index
      this.$refs.tabController2.curIndex = index
    },
    backClick () {
      this.$refs.scroll.scrollTo(0, 0, 1000)
    },
    onScroll (position) {
      // 1.判断BackTop是否显示
      this.isShowBackTop = (-position.y) > 1000

      // 2.决定tabControl是否吸顶(position: fixed)
      this.isTabFixed = (-position.y) > this.tabOffsetTop
    },
    loadMore () {
      this.getHomeGoods(currentType)
    },
    swiperImageLoad () {
      this.tabOffsetTop = this.$refs.tabController2.$el.offsetTop
    },
    /*
      网络请求
     */
    getHomeMultiData () {
      getHomeMultiData().then((result) => {
        this.banners = result.data.banner.list;
        this.recommends = result.data.recommend.list;
      })
    },
    getHomeGoods (type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(result => {
        this.goods[type].list.push(...result.data.list)
        this.goods[type].page += 1
        this.$refs.scroll.finishPullUp()
      })
    }
  }
};
</script>

<style scoped>
#home {
  height: 100vh;
  position: relative;
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}

.content {
  overflow: hidden;

  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}

.tab-controller {
  position: relative;
  z-index: 9;
}
</style>
