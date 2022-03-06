
````
renderPopver(h, {row, column}, val = "") {
  let value = val || row[column.property] || "";
  let value2 = value.replace(/<img.*?\/?/gi, " ");
  let length = value.length < 300 ? 300 : value.length < 700 ? 500 : 700;
  let text = <p domPropsInnerHTML = {value} />;
  let text2 = (
    <p
      domPropsInnerHTML = {value2}
      style = {{
        width: `${column.resizeWidth - 20}px`,
        overflow: "hidden",
        wordWrap: "break-word",
        wordBreak: "break-all",
      }}
    />
  );
  return (
    <aui-popover placement="top-start" width={length} trigger="hover" content={value}>
      {text}
      <div slot="reference">{text2}</div>
    </aui-popover>
  );
}

````


````
renderSelect(h, {row, column}) {
    let options = this[column.parsms.options];
    let result = options.map( v => {
      return <aui-option label={v.label} value={v.value} key={v.value} />
    });
    return (
      <aui-select
        value={
          !!row[column.property] ?
            row[column.property] :
            column?.params?.correlativeColumn ?
              row[column.params.correlativeColumn] :
              column?.params?.defaultVal !==null || column?.params?.defaultVal !== undefined ?
                column.params.defaultVal : ""
        }
        clearable={{!!column?.params.notClearable ? false : true}}
        onChange={val => {
          row[column.property] = val;
          isFunction(this.[column?.params?.editClosedCb]) &&
          this.[column?.params?.editClosedCb](options, val, row);
        }}
      >
        {result}
      </aui-select>
    );
  }

````

````
renderConclusionDesc(h, {row, column}) {
    return (
      <div class={this.isReportOnly ? "desc position-r" : "desc position-r editable"}>
        <TinymceHtmlRowDisplay
          class="pl-20"
          style={{
            overflow: "hidden",
            maxWidth: `${column.renderWidth - 45}px`
          }}
          v-model={row.conclusionDesc}
          isSingleline={this.columnOverflow}
        />
        <span
          class="g-operate position-a icon-edit hidden"
          title="点击编辑评估结论"
          onClick={() =>{
            this.handlesEdit(row);
            this.content = row.conclusionDesc || "";
            this.selfSummary = row.conclusionDecs;
          }}
        >
          <i class="el-icon-edit-outline" />
        </span>
      </div>
    );
  }

````



