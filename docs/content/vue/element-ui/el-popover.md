# el-popover

````
renderReportLog(h, params) {
    const { row, column } = params;
    const value = row[column.prop] === null ? "" : row[column.prop];
    const itemUnit = isNaN(parseFloat(value)) && isFinite(value) ? "" : row["itemUnit"];
    const logStr = row[column.prop + "Log"];
    const log = Object.assign({}, {itemValue: {}}, JSON.parse(logStr));
    return(
        <el-popover
            popper-class="popover-bg"
            placement="right"
            width="250"
            disabled={logStr === null}
        >
        <div>
            <p>修改人：{log.itemValue.user}</p>
            <p>修改时间：{log.itemValue.date}</p>
            <p>旧值：{log.itemValue.oldValue}</p>
            <p>IT值：{row.itemValueIt}</p>
        </div>
        <span slot="reference">{value + itemUnit}</span>
      </el-popover>
    )
}
````

````
<style lang="less" scoped>
  /deep/ .popover-bg {
    background-color: #303133 !important;
    color: #fff;
    .popper__arrow::after {
      border-right-color: #303133 !important;
    }
  }
</style>
````





  
  