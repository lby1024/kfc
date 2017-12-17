<template>
  <div id="app">
    <!-- 左侧导航nav -->
    <nav-left></nav-left>
    <!-- 右侧操控区域 -->
    <div class="main">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import NavLeft from "./components/comon/left-nav";

export default {
  name: "app",
  components: {
    NavLeft
  }
};
</script>

<style scoped>
	.main{
		position: absolute;
		left: 90px;
		right: 0px;
		background-color: gray;
		height: 100%;
	}
</style>
