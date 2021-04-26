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
      <div class="main-middle">
        <bordeBox class="influencers">
          <template #boxTitle>
            Influencers Spread Path
          </template>
          <template #boxMain>
            <div class="main">
              <div class="left-side">
                <img src="../assets/img/circle-list.png" alt="">
              </div>
              <div class="right-side">
                <div class="table-head">
                  <div class="rank">Rank</div>
                  <div class="kol">KOL</div>
                  <div class="fans">Fans</div>
                  <div class="index">KOL Index</div>
                </div>
                <div class="table-body" ref="tableBody">
                  <vue-seamless-scroll :data="rankList" class="seamless-warp" :class-option="scollOptions">
                    <ul class="item">
                      <div class="th" ref="tableTh" v-for="(item, index) in rankList" :key="index">
                        <div class="rank">{{index+1}}</div>
                        <div class="kol">
                          <img :src="item.headImg" alt="">
                          {{item.kol}}
                        </div>
                        <div class="fans">
                          {{item.fans}}
                        </div>
                        <div class="index">
                          {{item.kolIndex}}
                        </div>
                      </div>
                    </ul>
                  </vue-seamless-scroll>
                </div>
              </div>
            </div>
          </template>
        </bordeBox>
      </div>
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
        <bordeBox class="social-quotes">
          <template #boxTitle>
            Social Quotes
          </template>
          <template #boxMain>
            <div class="main">
              <div class="news-item" v-for="(item, index) in newsList" :key="index" @click="goPage(item)">
                <div class="left-side">
                  <div class="index">{{index+1}}</div>
                </div>
                <div class="right-side">
                  <p class="title">
                    {{item.desc.slice(0,20)}}
                    <img src="../assets/img/Shape.png" alt="">
                    <span class="hot-text">{{item.hot}}</span>
                  </p>
                  <p class="subtitle">
                    09:00 财经网站
                  </p>
                  <p class="desc">
                    {{item.desc.slice(0,80)}}
                  </p>
                </div>
              </div>
            </div>
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
import { rankList, echarts05Data } from '@/assets/js/data'
import newsList from '@/assets/js/news.json'
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
      echarts05Data: echarts05Data,
      rankList: rankList,
      scollOptions:{
        step: 1
      },
      newsList: newsList
    }
  },
  mounted () {
    this.resizeFn()
    window.onresize = this.resizeFn

    this.currentNumber()

    this.shuffle()
    setInterval(() => {
      this.loop()
    },5000)
    this.loop()
  },
  methods: {
    goPage(item){
      window.open(item.url)
    },
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
        randomDate.push(dayjs().subtract(4*i, 'h').format('YYYY.MM.DD HH:mm:ss'))
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
          height: 450px;
          .current-number-chart{
            width: 50%;
            height: 50%;
          }
        }
      }
    }
    &-middle{
      display: flex;
      flex-direction: column-reverse;
      margin: 0 10px;
      flex: 1.6;
      .influencers{
        width: 100%;
        height: 496px;
        color: #fff;
        .main{
          width: 100%;
          height: 100%;
          .left-side{
            padding: 10px;
            img{
              animation:transparency 4s linear infinite;
              @keyframes transparency {
                0%{
                  opacity: 1;
                }
                20%{
                  opacity: .8;
                }
                35%{
                  opacity: .6;
                }
                50%{
                  opacity: .3;
                }
                65%{
                  opacity: .5;
                }
                80%{
                  opacity: .8;
                }
                100%{
                  opacity: 1;
                }
              }
            }
          }
          .right-side{
            display: flex;
            flex-direction: column;
            flex: 1;
            .table-head{
              display: flex;
              color: #00aeff;
              flex: none;
              height: 50px;
              align-items: center;
              background: rgba(2,58,160,.28);
              &>div{
                flex: 1;
              }
              .kol{
                flex: 2;
              }
              .index{
                flex: 1.3;
              }
            }
            .table-body{
              display: flex;
              flex-direction: column;
              text-align: left;
              flex: 1;
              overflow: hidden;
              .th{
                display: flex;
                text-align: left;
                height: 50px;
                flex: none;
                align-items: center;
                &:nth-child(2n){
                  background: rgba(2,58,160,.28);
                }
                .rank{
                  color: #00aeff;
                  width: 40px;
                  text-align: center;
                }
                .kol{
                  display: flex;
                  align-items: center;
                  flex: 1.8;
                  font-size: 14px;
                  img{
                    margin-right: 10px;
                  }
                }
                .fans{
                  flex: 1;
                }
                .index{
                  flex: 1;
                  color: #16dffe;
                }
              }
            }
          }
        }
      }
    }
    &-right{
      flex: 1;
      .word-cloud{
        margin-bottom: 20px;
        height: 434px;
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

      .social-quotes{
        width: 100%;
        height: 488px;
        .main{
          display: flex;
          flex-direction: column;
          padding: 10px 20px;
          width: 100%;
          box-sizing: border-box;
          height: 100%;
          overflow: auto;
          .news-item{
            display: flex;
            padding: 10px 0;
            border-bottom: 1px solid rgba(0, 193, 249, 0.3);
            cursor: pointer;
            .left-side{
              display: flex;
              justify-content: center;
              flex: none;
              width: 40px;
              color: #fff;
              box-sizing: border-box;
              .index{
                display: flex;
                justify-content: center;
                align-items: center;
                font-size: 14px;
                height: 26px;
                width: 26px;
                background: url("../assets/img/Rectangle.png") no-repeat;
              }
            }
            .right-side{
              display: flex;
              flex-direction: column;
              .title{
                margin-bottom: 10px;
                display: flex;
                align-items: center;
                color: #008aff;
                img{
                  margin: 0 10px;
                }
                .hot-text{
                  color: #ca2434;
                }
              }
              .subtitle{
                margin-bottom: 8px;
                font-size: 14px;
                color: rgba(255,255,255,.4);
              }
              .desc{
                display: -webkit-box;
                -webkit-box-orient: vertical;
                -webkit-line-clamp: 2;
                overflow: hidden;
                color: rgba(255,255,255,.8);
              }
            }
          }
        }
      }
    }
  }

</style>
