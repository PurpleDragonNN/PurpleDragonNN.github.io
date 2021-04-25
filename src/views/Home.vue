<template>
  <div class="container" :style="resize">
    <div class="header">
      <p class="title">圣越32产业园</p>
    </div>
    <div class="main">
      <div class="main-left"></div>
      <div class="main-middle"></div>
      <div class="main-right"></div>
    </div>
  </div>
</template>

<script>
import bordeBox from '@/components/bordeBox.vue'
import 'echarts-liquidfill/src/liquidFill.js'
export default {
  name: 'Home',
  components: {
  },
  data () {
    return {
      resize: {},
      isCloseAll: false,
      isShowTips: false,
      toolTipContent: {},
      nowTime: '',
      activeVideo: '',
      isShowVideoDialog: false
    }
  },
  mounted () {
    this.resizeFn()
    window.onresize = this.resizeFn
    this.getEchartData()
    this.videoList.forEach(item => {
      this.$refs[item.name][0].addEventListener('play', () => {
        item.isPlay = true
      }, false);
    })
    setInterval(() => {
      let date = new Date()
      let week = new Date().getDay()
      let weekObj = {
        0: '天',
        1: '一',
        2: '二',
        3: '三',
        4: '四',
        5: '五',
        6: '六',
      }
      let month = date.getMonth()<10 ? '0'+(date.getMonth()+1) : date.getMonth()
      this.nowTime = `${month}-${date.getDate()}-${date.getFullYear()} 星期${weekObj[week]} ${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`
    },1000)
  },
  methods: {
    btnEvent () {
      this.$router.push({path: './second'})
    },
    play (val){
      val.isPlay = true
      this.$refs[val.name][0].play()
    },
    closeVideo () {
      this.isCloseAll = true
      this.videoList.forEach(item => {
        item.isPlay = false
      })
      this.$nextTick(() => {
        this.isCloseAll = false
      })
    },
    getEchartData () {
      this.yuchangfenData()
      this.roujiData()
      this.youjifeiData()
      this.fuhuaData()
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
