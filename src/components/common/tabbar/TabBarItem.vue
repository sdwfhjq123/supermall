<template>
  <!--所有的item都展示同一个图片, 同一个文字-->
  <div class="tab-bar-item" @click="click">
    <div v-if="!isActive">
      <slot name="item-icon"></slot>
    </div>
    <div v-else>
      <slot name="item-icon-active"></slot>
    </div>
    <div :style="activeStyle">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TabBarItem',
  props: {
    path: String,
    activeColor: {
      type: String,
      default: 'red'
    }
  },
  data() {
    return {
      // isActive: true
    }
  },
  computed: {
    isActive() {
      return this.$route.path.indexOf(this.path) != -1
    },
    activeStyle() {
      return this.isActive ? { color: this.activeColor } : {}
    }
  },
  methods: {
    click() {
      console.log(this.path);
      if (this.$route.path == this.path) return
      this.$router.replace(this.path)
    }
  }
}
</script>

<style>
.tab-bar-item {
  flex: 1;
  text-align: center;
  height: 49px;
  background-color: #f6f6f6;
  font-size: 14px;
}

.tab-bar-item img {
  width: 24px;
  height: 24px;
  margin-bottom: 2px;
  vertical-align: middle;
  margin-top: 3px;
}

.active {
  color: red;
}
</style>