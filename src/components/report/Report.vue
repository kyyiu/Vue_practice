<template>
  <div>
    <!-- 面包屑导航区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home'}">首页</el-breadcrumb-item>
      <el-breadcrumb-item >数据统计</el-breadcrumb-item>
      <el-breadcrumb-item >数据报表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
      <!-- 未echarts准备一个具备宽高的dom -->
      <div id="main" style="width: 600px;height: 400px;"></div>
    </el-card>
  </div>
</template>
<script>
import echarts from 'echarts'
import _ from 'lodash'
export default {
  data () {
    return {
      // 需要合并的选项
      options: {
        title: {
          text: '用户来源'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: '#E9EEF3'
            }
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        xAxis: [
          { boundaryGap: false }
        ],
        yAxis: [
          { type: 'value' }
        ]
      }
    }
  },
  created () {},
  // 此时页面上的元素已经被渲染完毕了
  async mounted () {
    var myChart = echarts.init(document.getElementById('main'))
    const { data: res } = await this.$http.get('reports/type/1')
    if (res.meta.status !== 200) {
      return this.$message.error('获取折线图数据失败')
    }
    const result = _.merge(res.data, this.options)
    myChart.setOption(result)
  },
  methods: {}
}
</script>

<style lang="less" scoped>
</style>
