<template>
  <div>
    <nav-bar>
      <div slot="left" class="back" @click="backClick">
        <img src="~assets/img/common/back.svg" alt />
      </div>
      <div slot="center" class="titles">
        <div
          v-for="(item,index) in titles"
          :key="item"
          class="title-item"
          :class="{active:index==curIndex}"
          @click="itemClick(index)"
        >{{item}}</div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
import NavBar from 'components/content/NavBar'

export default {
  name: 'DetailComponent',
  components: {
    NavBar
  },
  data () {
    return {
      titles: ['商品', '参数', '评论', '推荐'],
      curIndex: 0
    }
  },
  methods: {
    itemClick (index) {
      this.curIndex = index
    },
    backClick () {
      this.$router.go(-1)
    }
  }
}
</script>

<style scope>
.titles {
  display: flex;
  font-size: 13px;
  width: 200px;
}

.title-item {
  flex: 1;
}

.active {
  color: var(--color-tint);
}

.back {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}
</style>