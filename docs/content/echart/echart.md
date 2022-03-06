
````
<template>
  <div id="chart" ref="chart"/>
</template>

<script>
  import {dateFormat} from "@/../utils/common";

  export default {
    methods: {
      fetchData() {
        let api = "services/disRisk/getDtsTackleChart";
        let params = {
          reportId: this.$store.getters.reportId,
          pbiCode: this.$store.getters.pbiCode,
          reprotType: this.$store.getters.reprotType,
          serviceId: this.$store.getters.serviceId,
          stage: this.$store.getters.trStage,
        };
        this.$service.network.post(api, params).then(res => {
          if (res.status === 200 && res?.data?.status == "success") {
            let data = res.data.data;
            let weekEndDates = data.weekEndDates && data.weekEndDates.map(v => {
              let date = new Date(v);
              return dateFormat(date.setDate(date.getDate() - 1));
            });
            let obj = Object.assign({}, data, {weekEndDates});
            this.setData(obj);
          } else {
            this.$message({
              type: "error",
              message: "数据错误",
            });
          }
        });
      },

      setData(data) {
        let aColor = ["#f5222d", "#e6a23c", "#2bdadd", "#b6f7f8", "#4ad2ff", "#87cc9e"];
        let xAxisData = [];
        let series = [
          {
            field: "needTackleNums",
            name: "数据一",
            type: "line",
          },
          {
            field: "startedTackleNums",
            name: "数据二",
            type: "line",
          },
          {
            field: "unTackleNums",
            name: "数据三",
            type: "line",
          },
          {
            field: "finishedTackleNums",
            name: "数据四",
            type: "line",
          },
          {
            field: "newTackleWeekNums",
            name: "数据五",
            type: "bar",
            barWidth: 30,
            yAxisIndex: 1
          },
          {
            field: "finishedTackleWeekNums",
            name: "数据六",
            type: "bar",
            barWidth: 30,
            yAxisIndex: 1
          },
        ];
        xAxisData = data.weekEndDates;
        series = series.map((v, i) => {
          for (let k in data) {
            if (k == v.field) {
              return Object.assign({}, {data: data[v.field]}, {itemStyle: {normal: {color: aColor[i]}}});
            }
          }
        });
        this.drawChart({xAxisData, series});
      },

      drawChart({xAxisData, series}) {
        this.$nextTick(() => {
          let oChart = this.echart.init(document.querySelector("#chart"));
          this.setOption({
            chart: oChart,
            xAxisData,
            series
          });
        });
      },

      setOption({chart, legendData, xAxisData, series, title, tooltipFm}) {
        chart.setOption(
          {
            title: {
              subtext: "xxxx",
              text: title,
              left: "50%",
              top: "5%",
              containLabel: true,
            },
            tooltip: {
              trigger: "axis",
              axisPoninter: {
                type: "cross",
                label: {
                  backgroundColor: "#6a7985",
                }
              },
              formatter: tooltipFm || "",
            },
            grid: {
              left: "50px",
              right: "60px",
              bottom: "15%",
            },
            legend: {
              type: "scroll",
              oriend: "horizontal",
              right: "50px",
              top: "-5",
              textStyle: {
                color: "#333",
              },
              data: legendData,
            },
            xAxis: {
              data: xAxisData,
              type: "category",
              axisLabel: {
                interval: 0,
                rotate: 0,
              },
              axisTick: {
                show: false,
              },
            },
            yAxis: [
              {
                name: "折线",
                splitLine: {
                  show: false,
                },
                type: "value",
                minInterval: 1,
              },
              {
                name: "柱图",
                splitLine: {
                  show: false,
                },
                type: "value",
                minInterval: 1,
                axisLabel: {
                  formatter: "{value}%"
                }
              }
            ],
            series,
          },
          true
        );
      },

    },
    created() {
      this.fetchData();
    }
  }
</script>

<style scoped lang="less">

</style>
````









