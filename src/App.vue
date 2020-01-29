<template>
  <div id="app">
    <div id="main"
         style="width: 100%;height:800px;"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china.js'
import china from '../static/map/china.json'
import anhui from '../static/map/province/anhui.json'

export default {
  name: 'App',
  data () {
    return {
      myChart: undefined,
      option: {
        backgroundColor: '#f4f5f5',
        title: {
          text: 'Echarts3 中国地图下钻至县级',
          left: 'center',
          textStyle: {
            color: '#000',
            fontSize: 16,
            fontWeight: 'normal',
            fontFamily: 'Microsoft YaHei'
          }
        },
        tooltip: {
          trigger: 'item'
        },
        toolbox: {
          show: false,
          orient: 'vertical',
          left: 'right',
          top: 'center'
        },
        animationDuration: 1000,
        animationEasing: 'cubicOut',
        animationDurationUpdate: 1000
      }
    }
  },
  mounted () {
    let vm = this
    this.myChart = echarts.init(document.getElementById('main'))
    let d = []
    for (var i = 0; i < china.features.length; i++) {
      d.push({
        name: china.features[i].properties.name,
        value: '100'
      })
    }
    echarts.registerMap('china', china)
    this.renderMap('china', d)
    this.myChart.on('click', function (params) {
      echarts.registerMap('安徽', anhui)
      let d = []
      for (let i = 0; i < anhui.features.length; i++) {
        d.push({
          name: anhui.features[i].properties.name,
          value: '200'
        })
      }
      vm.renderMap('安徽', d)
    })
  },

  methods: {
    renderMap (map, data) {
      this.option.title.subtext = map
      this.option.series = [
        {
          name: map,
          type: 'map',
          mapType: map,
          roam: false,
          nameMap: {
            'china': '中国'
          },
          label: {
            normal: {
              show: true,
              textStyle: {
                color: '#999',
                fontSize: 13
              }
            },
            emphasis: {
              show: true,
              textStyle: {
                color: '#fff',
                fontSize: 13
              }
            }
          },
          itemStyle: {
            normal: {
              areaColor: '#323c48',
              borderColor: 'dodgerblue'
            },
            emphasis: {
              areaColor: 'darkorange'
            }
          },
          data: data
        }
      ]
      this.myChart.setOption(this.option)
    }
  }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
* {
  margin: 0;
  padding: 0;
}
body {
  font-family: Exo, "-apple-system", "Open Sans", HelveticaNeue-Light,
    "Helvetica Neue Light", "Helvetica Neue", "Hiragino Sans GB",
    "Microsoft YaHei", Helvetica, Arial, sans-serif;
  color: #333333;
}
</style>
