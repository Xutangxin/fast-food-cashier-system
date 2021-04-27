<template>
  <!--左侧导航栏-->
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-gouwuche2"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-jinrudianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-wo"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-gongnengjianyi"></i>
        <div>统计</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "leftNav",
};
</script>

<style scope>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.left-nav {
  float: left;
  width: 5%;
  height: 100%;
  color: white;
  font-size: 10px;
  background-color: skyblue;
}
.iconfont {
  font-size: 24px;
}
.left-nav ul {
  margin: 0;
  padding: 0;
}
.left-nav li {
  list-style: none;
  text-align: center;
  border-bottom: 1px solid lightgray;
  padding: 10px;
  cursor: pointer;
}
</style>