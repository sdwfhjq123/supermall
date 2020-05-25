<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>

    <scroll class="content" ref="scroll" :probe-type="3" :pull-up-load="true" @onScroll="onScroll">
      <home-swiper :banners="banners" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-controller class="tab-controller" :titles="['新款', '流行', '精选']" @tabClick="tabClick" />
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
      isShowBackTop: false
    }
  },
  created () {
    this.getHomeMultiData()
    this.getHomeGoods('new')
    this.getHomeGoods('pop')
    this.getHomeGoods('sell')
  },
  mounted () {
    this.$bus.$on('itemImageLoadCompleted', () => {
      this.$refs.scroll.refresh()
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
    },
    backClick () {
      this.$refs.scroll.scrollTo(0, 0, 1000)
    },
    onScroll (position) {
      this.isShowBackTop = -(position.y) > 1000
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
      })
    }

  }
};
</script>

<style scoped>
#home {
  padding-top: 44px;
  height: 100vh;
  position: relative;
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}

.tab-controller {
  position: sticky;
  top: 44px;
  z-index: 9;
}

.content {
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
}
</style>
