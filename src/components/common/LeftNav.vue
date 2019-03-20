<template>
  <div id="LeftNav">
    <ul>
      <li>
        <i class="icon iconfont icon-goumai"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-huiyuanqia"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-31tuwenxiangqing"></i>
        <div>统计</div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'leftNav',
  data() {
    return {};
  }
};
</script>
<style>
#LeftNav {
  color: #fff;
  font-size: 10px;
  height: 100%;
  background-color: #1d8ce0;
  float: left;
  width: 5%;
}
#LeftNav ul {
  margin: 0;
  padding: 0;
}
#LeftNav li {
  list-style: none;
  text-align: center;
  border-bottom: 1px solid #20a0ff;
  padding: 10px;
}
.iconfont {
  font-size: 24px;
}
</style>

