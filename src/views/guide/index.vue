<template>
    <el-row :gutter="20">
        <el-col :span="10" :gutter="20" class="leftEcharts" offset="1">
            <br>
            <div class="block">
                <el-date-picker
                style="width:80%"
                v-model="value1"
                type="monthrange"
                align="right"
                unlink-panels
                range-separator="至"
                start-placeholder="开始月份"
                end-placeholder="结束月份"
                value-format="yyyy-MM"
                :picker-options="pickerOptions"
                @change="handleDateChange">
                </el-date-picker>
            </div>
            <div class="hi" >
                <div>
                <div class="firstLine">
                    <div class="single">贷款业务</div>
                    <div>
                        <div class="leftArea">
                            <span class="areaTop"><strong>6770.49</strong></span><br>
                            <span class="areaBottom">成交金额（万美元）</span>
                        </div>
                        <div class="rightArea">
                            <span class="areaTop"><strong>269</strong></span><br>
                            <span>成交笔数（笔）</span>
                        </div>
                    </div>
                </div>
                <div class="secondLine">
                    <div class="single">国内业务</div>
                        <div>
                        <div class="leftArea">
                            <span class="areaTop"><strong>3376.57</strong></span><br>
                            <span class="areaBottom">成交金额（万元）</span>
                        </div>
                        <div class="rightArea">
                            <span class="areaTop"><strong>82</strong></span><br>
                            <span>成交笔数（笔）</span>
                        </div>
                    </div>
                </div>
                <div class="finalLine">
                    <div class="single">跨境业务</div>
                    <div>
                        <div class="leftArea">
                            <span class="areaTop"><strong>6266.60</strong></span><br>
                            <span class="areaBottom">成交金额（万美元）</span>
                        </div>
                        <div class="rightArea">
                            <span class="areaTop"><strong>187</strong></span><br>
                            <span>成交笔数（笔）</span>
                        </div>
                    </div>
                </div>
                </div>
            </div>
            <el-row style="width:500px;height:400px;" id="leftDownContainer"></el-row>
        </el-col>
        <el-col :span="10" :gutter="20" class="rightEcharts" offset="1">
            <br/>
            <el-row style="width:600px;height:400px;" id="rightTopContainer"></el-row>
            <el-switch
                v-model="value3"
                active-text="业务状态"
                inactive-text="绩效排名">
            </el-switch>
            <el-row type="flex" align="middle" class="paihangbang">
                <div v-show="!value3" class="son"  id="rightDownContainer"></div>
                <div v-show="value3"  class="son" id="rightDownContainer1"></div>
            </el-row>
        </el-col>
    </el-row>
</template>
<script>
export default {
  name: 'index',
  data() {
    return {
      msg: 'welcome to databoard',
      pickerOptions: {
        shortcuts: [{
          text: '本月',
          onClick(picker) {
            picker.$emit('pick')
          }
        }, {
          text: '今年至今',
          onClick(picker) {
            const end = new Date()
            const start = new Date(new Date().getFullYear(), 0)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近六个月',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setMonth(start.getMonth() - 6)
            picker.$emit('pick', [start, end])
          }
        }]
      },
      value1: ['2000-01', '2000-03'], // date
      value3: true,
      firstLine: {
        nameSingle: '贷款业务',
        leftArea: 1234.5,
        rightArea: 234
      },
      secondLine: {
        nameSingle: '国内业务',
        leftArea: 6434.5,
        rightArea: 155
      },
      lastLine: {
        nameSingle: '跨境业务',
        leftArea: 734.5,
        rightArea: 288
      },
      qianshu: '成交金额（万元）',
      bishu: '成交笔数（笔）'
    }
  },
  mounted() {
    // 请求数据
    this.drawLeftDown()
    this.drawRightTop()
    this.drawRightDown()
    this.drawRightDown1()
  },
  methods: {
    getDataAndsetTest() {
      const xAx = []
      const yAy = []
    },
    drawRightDown1() {
      const RightDown1Echart = this.$echarts.init(document.getElementById('rightDownContainer1'))
      window.onresize = function() {
        RightDown1Echart.resize()
      }
      const option = {
        title: {
          text: '',
          subtext: '',
          left: ''
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        legend: {
          left: 'right',
          top: 'center',
          orient: 'vertical',
          data: [
            '额度合同生成',
            '放款成功',
            '全量校验成功',
            '部门经理审核成功',
            '部门经理审核失败',
            '预登记提交成功',
            '担保费提交成功',
            '项目已解除'
          ]
        },
        toolbox: {
          show: true,
          feature: {
            mark: { show: true },
            dataView: { show: true, readOnly: false },
            restore: { show: true },
            saveAsImage: { show: true }
          }
        },
        series: [
          {
            name: '业务状态',
            type: 'pie',
            radius: [20, 140],
            center: ['45%', '50%'],
            roseType: 'radius',
            itemStyle: {
              borderRadius: 5
            },
            label: {
              show: false
            },
            emphasis: {
              label: {
                show: true
              }
            },
            data: [
              { value: 40, name: '额度合同生成' },
              { value: 33, name: '放款成功' },
              { value: 28, name: '全量校验成功' },
              { value: 22, name: '部门经理审核成功' },
              { value: 20, name: '部门经理审核失败' },
              { value: 15, name: '预登记提交成功' },
              { value: 12, name: '担保费提交成功' },
              { value: 10, name: '项目已解除' }
            ]
          }

        ]
      }
      option && RightDown1Echart.setOption(option)
    },
    drawRightTop() {
      const rightTopEchart = this.$echarts.init(document.getElementById('rightTopContainer'))
      window.onresize = function() {
        rightTopEchart.resize()
      }
      const option = {
        title: { text: '业务笔数' },
        legend: {},
        tooltip: {},
        dataset: {
          // 数据集中管理
          source: [
            ['data', '借款总人数'],
            ['202201', 43.3],
            ['202202', 83.1],
            ['202203', 86.4],
            ['202204', 72.4],
            ['202205', 67]
          ]
        },
        // 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
        xAxis: { type: 'category' },
        // 声明一个 Y 轴，数值轴。
        yAxis: {},
        // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
        series: [{ type: 'bar' }]
      }
      rightTopEchart.setOption(option)
    },
    drawLeftDown() {
      const leftDownEchart = this.$echarts.init(document.getElementById('leftDownContainer'))
      window.onresize = function() {
        leftDownEchart.resize()
      }
      const option = {
        title: { text: '业务金额' },
        legend: {},
        tooltip: {},
        dataset: {
          // 数据集中管理
          source: [
            ['data', '借款总金额'],
            ['202201', 43.3],
            ['202202', 83.1],
            ['202203', 86.4],
            ['202204', 72.4]
          ]
        },
        // 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
        xAxis: { type: 'category' },
        // 声明一个 Y 轴，数值轴。
        yAxis: {},
        // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
        series: [{ type: 'bar' }]
      }
      leftDownEchart.setOption(option)
    },
    drawRightDown() {
      // 初始化排行榜
      const rightDownEchart = this.$echarts.init(document.getElementById('rightDownContainer'))
      window.onresize = function() {
        rightDownEchart.resize()
      }
      const option = {
        title: {},
        dataset: [
          {
            dimensions: ['name', 'age', 'profession', 'score', 'date'],
            source: [
              ['Hannah ', 41, 'Engineer', 314, '2011-02-12'],
              ['Zhao ', 20, 'Teacher', 351, '2011-03-01'],
              ['Jasmin  ', 52, 'Musician', 287, '2011-02-14'],
              ['Li Lei', 37, 'Teacher', 219, '2011-02-18'],
              ['Karle ', 25, 'Engineer', 253, '2011-04-02'],
              ['Adrian ', 19, 'Teacher', 40, '2011-01-16'],
              ['Mia ', 71, 'Engineer', 165, '2011-03-19'],
              ['Böhm ', 36, 'Musician', 318, '2011-02-24']
            ]
          },
          {
            transform: {
              type: 'sort',
              config: { dimension: 'score', order: 'desc' }
            }
          }
        ],
        color: [
          '#4CD964',
          '#5AC8FA',
          '#007AFF',
          '#5856D6',
          '#FF2D70',
          '#FF3B30',
          '#FF9500',
          '#FFCC00',
          '#8E8E93'
        ],
        xAxis: {
        },
        yAxis: {
          inverse: true,
          type: 'category',
          axisLabel: { interval: 0, rotate: 30 }
        },
        series: {
          type: 'bar',
          encode: { x: 'score', y: 'name' },
          datasetIndex: 1,
          colorBy: 'data'
        }
      }
      option && rightDownEchart.setOption(option)
    },
    handleDateChange() {
      console.log('date change', this.value1)// 请求数据
    }
  }
}
</script>
<style>
    .paihangbang{
        width:650px;
        height:450px;
        display: flex;
    }
    .son{
        width: 600px;
        height: 400px;
        margin: auto;
    }
    .parent{
        text-align: center;
        display: flex;
    }
    .children{
        margin: auto;
    }
    p{
        font-size:x-large;
        color: #409EFF;
        margin: auto;
        font-family: 'Times New Roman', Times, serif;
    }
   .hi {
        /* width: 550px; */
        height: 400px;
        /* background-color: rgb(99, 77, 77); */
    }

    h1{
        font-size: 20px;
        display: inline;
    }

    .right {
        display: inline-block;
        margin-left: 180px;
    }

    .firstLine {
        height: 100px;
        /* width: 550px;  */
        display: flex;
        background-color: rgb(174, 194, 235);
        /* 距上边盒子的距离 */
        margin-top: 10px;
        /* 圆角属性 */
        border-radius: 8px;
    }

    .single {
        width: 30px;
        height: 100px;
        float: left;
        color: white;
        text-align: center;
        padding-top: 15px;
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }
    .firstLine .single{
        background-color: rgb(70, 70, 224);
    }

    .secondLine .single{
        background-color: orange;
    }

    .finalLine .single{
        background-color: red;
    }

    .leftArea {
        /* width: 260px; */
        width: 50%;
        height: 80px;
        float: left;
        left: 30px;
        vertical-align: center;
        text-align: center;
        padding-top: 20px;
    }
    .areaTop {
        font-size: 20px;
    }
    .rightArea {
        /* width: 200px; */
        width: 50%;
        height: 80px;
        padding: 0 50px;
        float: right;
        right: 0;
        text-align: center;
        /* 垂直居中没整明白 */
        padding-top: 20px;
    }

    .secondLine {
        height: 100px;
        /* width: 550px; */
        display: flex;
        background-color: rgb(235, 219, 188);
        margin-top: 10px;
        border-radius: 8px;
    }

    .finalLine {
        height: 100px;
        /* width: 550px; */
        display: flex;
        background-color: rgb(223, 186, 186);
        margin-top: 10px;
        border-radius: 8px;
    }
</style>
