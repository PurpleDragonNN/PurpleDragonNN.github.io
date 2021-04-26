<template>
  <div class="container" :style="resize">
    <div class="header">
      <p class="title"></p>
    </div>
    <div class="main">
      <div class="main-left">
        <bordeBox class="buzz-line">
          <template #boxTitle>
            Buzz Line
          </template>
          <template #boxMain>
            <div ref="buzzLine" class="buzz-line-chart"></div>
          </template>
        </bordeBox>
        <bordeBox class="current-number">
          <template #boxTitle>
            Current Number
          </template>
          <template #boxMain>
            <div class="main">
              <div ref="currentNumber0" class="current-number-chart"></div>
              <div ref="currentNumber1" class="current-number-chart"></div>
              <div ref="currentNumber2" class="current-number-chart"></div>
            </div>
          </template>
        </bordeBox>
      </div>
      <div class="main-middle"></div>
      <div class="main-right">
        <bordeBox class="word-cloud">
          <template #boxTitle>
            Topics Words Cloud
          </template>
          <template #boxMain>
            <div class="search-bar">
              <input type="text" class="search-input">
              <div class="search-btn">筛选</div>
            </div>
            <word-cloud-chart
              ref="wordCloudChart"
              id="echarts05"
              :data="echarts05Data"
            />
          </template>
        </bordeBox>
      </div>
    </div>
  </div>
</template>

<script>
import bordeBox from '@/components/bordeBox.vue'
import WordCloudChart from '@/components/WordCloudChart'
import 'echarts-liquidfill/src/liquidFill.js'
import dayjs from 'dayjs'
const wordSize = [15000,10081,9386,7500,7500,6500,6500,6000,4500,3800,3000,2500,2300,2000,1900,1800]
export default {
  name: 'Home',
  components: {
    WordCloudChart,
    bordeBox
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
          /*textStyle: {
            color: '#00eaff'
          },*/
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

    this.currentNumber()

    this.shuffle()
    /*setInterval(() => {
      this.loop()
    },5000)*/
    this.loop()
  },
  methods: {
    loop () {
      let randomArr = wordSize.shuffle()
      this.echarts05Data.forEach((item, index, arr) => {
        arr[index].value = randomArr[index]
      })
      if (this.$refs.wordCloudChart) {
        this.$refs.wordCloudChart.initChart()
      }
      this.buzzLineChart()
    },
    buzzLineChart () {
      let buzzLine = this.$echarts.init(this.$refs.buzzLine);
      let randomArr = [], randomDate = []
      for (let i = 0; i < 20; i++) {
        randomArr.push(Math.round(Math.random() * 1000000))
        randomDate.push(dayjs().format('YYYY.MM.DD HH:mm:ss'))
      }
      let option = {
        tooltip: {
          trigger: 'axis',
          borderColor: 'none',
          extraCssText:'width:250px;height:120px; background: rgba(3, 56, 125, .8);white-space: pre-wrap;',
          formatter: () => {
            return `<p style="color: #0071d7;font-size: 16px">这是一段新闻文案标题文案</p><p style="color: #999;font-size: 12px">09:00 财经网站</p><p style="color: #fff;font-size: 14px;width: 100%">这是一段新闻推送这是一段新闻推送这是一段新闻推送这是一段新闻推送这是一段新闻推送</p>`
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          axisLabel:{
            show: true,
            rotate:90,
            color: '#fff'
          },
          data: randomDate
        },
        yAxis: {
          type: 'value',
          axisLabel:{
            show: true,
            color: '#fff'
          },
          splitLine: {
            show: true,
            lineStyle:{
              color: '#fff',
              width: 1,
              type: 'solid',
              opacity: 0.2
            }
          }
        },
        grid: {
          width: '82%',
          height: '55%',
          left: 70,
          top:30
        },
        series: [{
          data: randomArr,
          type: 'line',
          areaStyle: {},
          color: new this.$echarts.graphic.LinearGradient(
                  0, 0, 0, 1,
                  [{
                    offset: 0,
                    color: '#00d2fd'
                  },
                    {
                      offset: 1,
                      color: 'rgba(0, 211, 255, .2)'
                    }
                  ]
          )
        }]
      }
      buzzLine.setOption(option);
    },
    currentNumber () {
      let currentNumber0 = this.$echarts.init(this.$refs.currentNumber0);
      let currentNumber1 = this.$echarts.init(this.$refs.currentNumber1);
      let currentNumber2 = this.$echarts.init(this.$refs.currentNumber2);
      let setting = [
        {
          color: '#0073e8',
          title: 'Buzz',
        },
        {
          color: '#a45efa',
          title: 'Coverage',
        },
        {
          color: '#00c1f9',
          title: 'Sentiment NSR',
        },
      ]
      let option = (item) => ({
        series: [{
          type: 'gauge',
          radius: '110%',
          center: ['50%', '50%'],
          startAngle: 220,
          endAngle: -40,
          min: 0,
          max: 100,
          splitNumber: 10,
          splitLine: {
            show:false
          },
          axisLine: {
            show: false,
          },
          pointer: {
            width: 1,
            itemStyle: {
              color: item.color
            },
          },
          anchor: {
            show: true,
            showAbove: true,
            size: 10,
            itemStyle: {
              color: 'none',
              borderWidth: 2,
              borderColor: item.color
            }
          },
          axisTick: {
            length: 12,
            lineStyle: {
              color: item.color,
              width: 2
            }
          },

          axisLabel: {
            color: '#fff',
            fontSize: 16,
            distance: 20,

          },
          title: {
            offsetCenter: [0, '60%'],
            fontSize: 16,
            color: '#fff'
          },
          detail: {
            fontSize: 18,
            offsetCenter: [0, '30%'],
            valueAnimation: true,
            formatter: function (value) {
              return value + '%';
            },
            color: item.color
          },
          data: [{
            value: 30,
            name: item.title
          }]
        }]
      })
      currentNumber0.setOption(option(setting[0]));
      currentNumber1.setOption(option(setting[1]));
      currentNumber2.setOption(option(setting[2]));
    },
    // 数组打乱顺序方法
    shuffle (){
      Array.prototype.shuffle = function() {
        var array = this;
        var m = array.length,
                t, i;
        while (m) {
          i = Math.floor(Math.random() * m--);
          t = array[m];
          array[m] = array[i];
          array[i] = t;
        }
        return array;
      }
    },
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
@import "../assets/css/common";
  .main{
    position: relative;
    display: flex;
    flex: 1;
    width: 100%;
    &-left{
      flex: 1;
      .buzz-line{
        width: 100%;
        .buzz-line-chart{
          width: 100%;
          height: 400px;
        }
      }
      .current-number{
        width: 100%;
        .main{
          display: flex;
          flex-wrap: wrap;
          justify-content: center;
          width: 100%;
          height: 400px;
          .current-number-chart{
            width: 50%;
            height: 50%;
          }
        }
      }
    }
    &-middle{
      margin: 0 10px;
      flex: 1.5;
    }
    &-right{
      flex: 1;
      .word-cloud{
        .search-bar{
          display: flex;
          align-items: center;
          padding: 10px 40px 0 30px;
          .search-input{
            flex: 1;
            height: 33px;
            border-radius: 5px;
            background: rgba(4, 46, 86, .3);
            outline: none;
            border: none;
          }
          .search-btn{
            margin-left: 10px;
            flex: none;
            padding: 4px 8px;
            border-radius: 8px;
            background: #007eff;
            cursor: pointer;
            color: #fff;
          }
        }
      }
    }
  }

</style>
