<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
@import "../src/assets/css/reset.css";
@font-face {
  font-family: 'PorscheNext-Regular';
  src: url("./assets/font/PorscheNext-Regular.otf");
}
html{
  width: 100%;
  height: 100%;
}
body{
  font-family: 'PorscheNext-Regular', 'SimHei',Microsoft YaHei;
  width: 100%;
  height: 100%;
  #app{
    width: 100%;
    height: 100%;
  }
}

</style>
