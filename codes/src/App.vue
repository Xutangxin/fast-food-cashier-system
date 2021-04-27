<template>
  <div id="app">
    <!-- 左侧导航栏 -->
    <left-nav></left-nav>
    <!-- 主体区域 pos组件-->
    <div class="main">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import leftNav from "./components/common/leftNav";
export default {
  name: "App",
  components: {
    leftNav,
  },
};
</script>

<style>
#app {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Micorsoft YaHei", "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.main {
  float: left;
  width: 95%;
  height: 100%;
  overflow: auto;
}
</style>
