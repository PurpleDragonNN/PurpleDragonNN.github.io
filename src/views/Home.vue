<template>
  <div class="container" :style="resize">
    <div class="header">
      <p class="title">圣越32产业园</p>
    </div>
    <div class="main">
      <div class="main-left">
        <word-cloud-chart
            id="echarts05"
            :data="echarts05Data"
        />
      </div>
      <div class="main-middle"></div>
      <div class="main-right"></div>
    </div>
  </div>
</template>

<script>
import bordeBox from '@/components/bordeBox.vue'
import WordCloudChart from '@/components/WordCloudChart'
import 'echarts-liquidfill/src/liquidFill.js'
export default {
  name: 'Home',
  components: {
    WordCloudChart
  },
  data () {
    return {
      resize: {},
      isCloseAll: false,
      isShowTips: false,
      toolTipContent: {},
      nowTime: '',
      activeVideo: '',
      isShowVideoDialog: false,
      echarts05Data: [
        {
          name: "吴亦凡",
          textStyle: {
            // Color can be a callback function or a color string
            color: '#00eaff'
          },
          value: 15000
        },
        {
          name: "B站",
          value: 10081
        },
        {
          name: "微博",
          value: 9386
        },
        {
          name: "汽车之家",
          value: 7500
        },
        {
          name: "旭子工作室",
          value: 7500
        },
        {
          name: "抖音",
          value: 6500
        },
        {
          name: "老司机出品",
          value: 6500
        },
        {
          name: "懂车帝",
          value: 6000
        },
        {
          name: "合肥庐阳保时捷中心",
          value: 4500
        },
        {
          name: "老司机汽车",
          value: 3800
        },
        {
          name: "萝卜报告",
          value: 3000
        },
        {
          name: "易车网",
          value: 2500
        },
        {
          name: "韩路",
          value: 2300
        },
        {
          name: "ranta",
          value: 2000
        },
        {
          name: "萝卜报告",
          value: 1900
        },
        {
          name: "现代化经济体系",
          value: 1800
        },
        /*{
          name: "国防动员",
          value: 1700
        },
        {
          name: "信息化战争",
          value: 1600
        },
        {
          name: "局部战争",
          value: 1500
        },
        {
          name: "教育",
          value: 1200
        },
        {
          name: "职业教育",
          value: 1100
        },
        {
          name: "兵法",
          value: 900
        },
        {
          name: "一国两制",
          value: 800
        },
        {
          name: "特点社会主义思想",
          value: 700
        },*/
      ]
    }
  },
  mounted () {
    this.resizeFn()
    window.onresize = this.resizeFn

    // this.echarts05Data.
  },
  methods: {
    // 自适应
    resizeFn () {
      let win_w = document.documentElement.clientWidth ||  document.body.clientWidth
      let win_h = document.documentElement.clientHeight || document.body.clientHeight
      let ratioX = win_w / 1920
      let ratioY = win_h / 1080;
      this.resize = {
        transform: "scale(" + ratioX + ", " + ratioY + ")",
        transformOrigin: "left top",
      }
    },
  }
}
</script>
<style scoped lang="scss">
@import "src/assets/css/common";
  .main{
    position: relative;
    display: flex;
    flex: 1;
    width: 100%;
    &-left{
      flex: 1;
    }
    &-middle{
      margin: 0 10px;
      flex: 1.5;
    }
    &-right{
      flex: 1;
    }
  }

</style>
