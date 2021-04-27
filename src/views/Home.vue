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
                  <div class="rank">Platform</div>
                  <div class="kol">KOL</div>
                  <div class="fans">Fans</div>
                  <div class="index">KOL Index</div>
                </div>
                <div class="table-body" ref="tableBody">
                  <vue-seamless-scroll :data="rankList()" class="seamless-warp" :class-option="scollOptions">
                    <ul class="item">
                      <div class="th" ref="tableTh" v-for="(item, index) in rankList()" :key="index">
                        <div class="rank">{{item.Platform}}</div>
                        <div class="kol">
                          <img src="../assets/img/question.png" alt="">
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
            <!--<div class="search-bar">
              <input type="text" class="search-input">
              <div class="search-btn">筛选</div>
            </div>
            <word-cloud-chart
              id="echarts05"
              :data="echarts05Data"
            />-->
            <img class="word-cloud_img" :src="wordCloudUrl" alt="">
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
                    微博
                    <img src="../assets/img/Shape.png" alt="">
                    <span class="hot-text">{{item.hot}}</span>
                  </p>
                  <p class="subtitle">
                    09:00
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
// import WordCloudChart from '@/components/WordCloudChart'
import 'echarts-liquidfill/src/liquidFill.js'
import dayjs from 'dayjs'
import { rankList, echarts05Data } from '@/assets/js/data'
import newsList from '@/assets/js/news.json'
import buzzJson from '@/assets/js/buzz.json'
import buzzLineJson from '@/assets/js/buzzLine.json'
export default {
  name: 'Home',
  components: {
    // WordCloudChart,
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
      newsList: newsList,
      wordCloudUrl: require('../assets/img/wordCloud/1.png')
    }
  },
  mounted () {
    this.resizeFn()
    window.onresize = this.resizeFn

    this.currentNumber()
    this.buzzLineChart()
    let loop = 1,urls = [
      require('../assets/img/wordCloud/1.png'),
      require('../assets/img/wordCloud/2.png'),
      require('../assets/img/wordCloud/3.png'),
      require('../assets/img/wordCloud/4.png'),
      require('../assets/img/wordCloud/5.png'),
    ]
    setInterval(() => {
      this.wordCloudUrl = urls[loop]
      loop++
      if (loop>4) loop = 0
    }, 5000)
  },
  methods: {
    goPage(item){
      window.open(item.url)
    },
    buzzLineChart () {
      let buzzLine = this.$echarts.init(this.$refs.buzzLine);
      let option = (randomArr) => ({
        tooltip: {
          trigger: 'axis',
          borderColor: 'none',
          extraCssText:'width:200px;height:100px; background: rgba(3, 56, 125, .8);white-space: pre-wrap;',
          formatter: (data) => {
            let active = randomArr.find(item => {
              return data[0].axisValue === item.time
            })
            if (!active.news) return ''
            return `<p style="color: #0071d7;font-size: 16px">${active.title}</p><p style="color: #999;font-size: 12px">${active.time}</p><p style="color: #fff;font-size: 14px;width: 100%;display: -webkit-box;-webkit-box-orient: vertical;-webkit-line-clamp: 2;overflow: hidden;">${active.news.slice(0,40)}</p>`
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
          data: randomArr.map(item => item.time)
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
          data: randomArr.map(item => item.Buzz),
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
      })
      let randomArr = buzzLineJson.slice(0,20)
      buzzLine.setOption(option(randomArr));

      let totalLine = buzzLineJson.length,loop = 0
      setInterval(() => {
        randomArr = buzzLineJson.slice(loop, loop+20)
        buzzLine.setOption(option(randomArr));
        loop++
        if (loop >= totalLine) loop = 0
      },4000)
    },
    currentNumber () {
      let currentNumber0 = this.$echarts.init(this.$refs.currentNumber0);
      let currentNumber1 = this.$echarts.init(this.$refs.currentNumber1);
      let currentNumber2 = this.$echarts.init(this.$refs.currentNumber2);
      let setting = [
        {
          color: '#0073e8',
          title: 'Buzz',
          value: buzzJson.buzz[0],
          max: Math.max(...buzzJson.buzz)
        },
        {
          color: '#a45efa',
          title: 'Coverage',
          value: buzzJson.coverage[0],
          max: Math.max(...buzzJson.coverage)
        },
        {
          color: '#00c1f9',
          title: 'Sentiment NSR',
          value: buzzJson.NSR[0],
          max: Math.max(...buzzJson.NSR)
        },
      ]
      let option = (item) => ({
        series: [{
          type: 'gauge',
          radius: '120%',
          center: ['50%', '55%'],
          startAngle: 220,
          endAngle: -40,
          min: 0,
          max: item.max,
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
            fontSize: 12,
            distance: 16,
            formatter: (val) => {
              if (item.title !== 'Sentiment NSR') {
                return (val/10000).toFixed(1)+'w'
              }
              return val
            },
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
              if (item.title !== 'Sentiment NSR') {
                return (value/10000).toFixed(2)+'w'
              }
              return (value/item.max*100).toFixed(2) + '%';
            },
            color: item.color
          },
          data: [{
            value: item.value,
            name: item.title
          }]
        }]
      })
      let len = buzzJson.buzz.length, i = 1
      setInterval(function () {
        setting.forEach((item, index) => {
          item.value = buzzJson[Object.keys(buzzJson)[index]][i]
        })
        i++
        if (i >= len) i = 1
        currentNumber0.setOption(option(setting[0]));
        currentNumber1.setOption(option(setting[1]));
        currentNumber2.setOption(option(setting[2]));
      }, 1000);
      currentNumber0.setOption(option(setting[0]));
      currentNumber1.setOption(option(setting[1]));
      currentNumber2.setOption(option(setting[2]));
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
              .rank{
                flex: 1.3;
                text-align: center;
              }
              .kol{
                flex: 1.8;
              }
              .fans{
                flex: 0.8;
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
                height: 60px;
                flex: none;
                align-items: center;
                &:nth-child(2n){
                  background: rgba(2,58,160,.28);
                }
                .rank{
                  padding-right: 10px;
                  color: #00aeff;
                  width: 80px;
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
            color: #fff;
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
        &_img{
          margin: 18px auto;
          display: block;
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
