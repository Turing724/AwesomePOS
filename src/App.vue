<template>
  <div id="app">
    <left-nav></left-nav>
    <div class="main">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import leftNav from '@/components/common/LeftNav';
export default {
  name: 'App',
  components: {
    leftNav
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.main {
  float: left;
  width: 95%;
  background-color: #eff2f7;
  height: 100%;
  overflow: auto;
}
</style>
