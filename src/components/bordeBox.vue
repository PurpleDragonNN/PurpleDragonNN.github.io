<template>
  <div class="border-box">
    <div class="box-header">
      <img src="@/assets/img/triangle.png" alt="">
      <div class="box-title">
        <slot name="boxTitle"></slot>
      </div>
      <div class="bg-bar"></div>
    </div>
    <div class="box-main" :class="{'bg-border': !noBorder}">
      <slot name="boxMain"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    noBorder: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
  $fontColor: #00d7ff;
  .border-box{
  display: flex;
  flex-direction: column;
    margin-bottom: 10px;
  .box-header{
    display: flex;
    align-items: center;
    padding-left: 16px;
    width: 100%;
    height: 44px;
    line-height: 44px;
    color: $fontColor;
    font-size: 20px;
    box-sizing: border-box;
    background: rgba(4, 46, 86, .1);
    border-top: 1px solid rgba(0, 193, 249, .8);
    img{
      flex: none;
      width: 16px;
      height: 31px;
    }
    .box-title{
      padding: 0 10px;
      flex: none;
    }
    .bg-bar{
      flex: 1;
      height: 23px;
      background: rgba(4, 46, 86, .3);
    }
  }
  .box-main{
    flex: 1;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: rgba(1,1,1,.2);
  }
  .bg-border{
  }
}
</style>
