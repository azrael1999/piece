# el-table

````
//item of column Array 
{
  prop: "qualityReason",
  label: "是否研发质量原因",
  headerAlign: "left",
  width: 100,
  edit: true,
  editor: this.renderBooleanEdit,
  render: this.renderBoolean,
  exportOpt: {
    render: ({rowData, fieldName}) => {
      return rowData[fieldName] === true ? "是" : rowData[fieldName] === false ? "否" : "";
    },
  },
  share: {
    render: ({rowData, fieldName}) => {
      return rowData[fieldName] === true ? "是" : rowData[fieldName] === false ? "否" : "";
    },
  },
}


renderBooleanEdit(h, {row, column}) {
  let property = column.property + "Edit";
  render (
    <el-select
      style="width: calc(100% + 8px); left: -4px"
      value={row[property].value}
      size="mini"
      placeholder="请选择"
      onBlur={() => {
        this.blurCheck(row[property])
      }}
      onChange={(val) => {
        row[property].value = val;
        row[column.property] = val;
        this.changeSelect(row[property]);
        this.saveTableData(column.property, val);
      }}
    >
      {
        this.optionBoolean.map(t => {
          return <el-option label={t.label} value={t.value} />;
        })
      }
    </el-select>
  );
}


renderBoolean(h, {row, column}) {
  let value = row[column.property] === true ? "是" : row[column.property] === false ? "否" : "";
  return <span>{value}</span>;
}

````





















  
  