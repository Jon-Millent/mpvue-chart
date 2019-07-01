<template>
  <div class="michart">
    <mpvue-echarts v-if="isOnLoad" :echarts="echarts" :onInit="onInit" :canvasId="canvasId" />
  </div>
</template>

<script>
  import echarts from 'echarts'
  import mpvueEcharts from 'mpvue-echarts'
  let chart = null
  let echartsArr = {}


  export default {

    components: {
      mpvueEcharts
    },
    props: {
      config: {
        default: {}
      }
    },
    data(){
      return {
        echarts: echarts,
        canvasId: '',
        chart: null,
        onInit: this.initChart,
        isOnLoad: false,
      }
    },
    mounted(){
      this.canvasId = 'michart' + new Date().getTime() + Math.floor(Math.random() * 100) + Math.floor(Math.random() * 100)
      this.isOnLoad = true
    },
    methods: {
      onLoad(){

      },
      initChart(canvas, width, height){

        echartsArr[this.canvasId] = echarts.init(canvas, null, {
          width: width,
          height: height
        });
        canvas.setChart(echartsArr[this.canvasId]);

        echartsArr[this.canvasId].setOption(this.config);

        console.log(echartsArr, 'echartsArr')

        return echartsArr[this.canvasId]; // 返回 chart 后可以自动绑定触摸操作

      },
      updateChart(newValue){

        let chart = echartsArr[this.canvasId]

        if(chart && newValue) {
          chart.setOption(newValue)
        }

      }
    },

    destroy(){
      if(echartsArr[this.canvasId]) {
        delete echartsArr[this.canvasId]
      }
    }
  }
</script>

<style>
  .michart {
    width: 100%;
    height: 300px;
  }
</style>
