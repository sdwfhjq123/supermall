<template>
  <div class="tab-controller">
    <div
      class="tab-controller-item"
      v-for="(item, index) in titles"
      :key="(item, index)"
      :class="{ active: index === curIndex }"
      @click="itemClick(index)"
    >
      <span>{{ item }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TabController",
  props: {
    titles: {
      type: Array,
      default () {
        return [];
      },
    },
  },
  data () {
    return {
      curIndex: 0,
    };
  },
  methods: {
    itemClick (index) {
      this.curIndex = index;
      this.$emit('tabClick', index)
    },
  },
};
</script>

<style>
.tab-controller {
  display: flex;
  text-align: center;
  font-size: 15px;
  height: 40px;
  line-height: 40px;
  position: sticky;
  top: 44px;
  background-color: white;
}

.tab-controller-item {
  flex: 1;
}

.tab-controller-item span {
  padding: 5px;
}

.active {
  color: var(--color-high-text);
}

.active span {
  border-bottom: 3px solid var(--color-tint);
}
</style>
