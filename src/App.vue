<template>
  <div id="app">
    <main-tab-bar class="main-tab-bar"/>
    <router-view></router-view>
  </div>
</template>

<script>
import MainTabBar from '@/components/content/MainTabBar'

export default {
  name: 'App',
  components: {
    MainTabBar
  }
}
</script>

<style>
@import "./assets/css/base.css";

.main-tab-bar{
  z-index: 10;
}
</style>
