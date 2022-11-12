# element-ui杂项

> + 事件中传递自定义参数

如何在element-UI 组件的change事件中传递自定义参数


以select为例，如果select写在循环里，触发change事件时可能不只需要传递被选中项的值，还要传递index过去，来改变同一循环中的其他标签的状态。
下面这样写是无效的：
@change="changeStatus(val, index)"
````
//无效, 无法获取第二个参数 index
<div v-for="(item,index) in itemList">
  <el-select v-model="item.value" @change="changeStatus(val, index)">
    <el-option v-for="op in options"
               :key="op.key"
               :label="op.label"
               :value="op.label"/>
  </el-select>
</div>
````

这样再封装一层就可以了：
@change="((val)=>{changeStatus(val, index)})"

````
<div v-for="(item,index) in itemList">
  <el-select v-model="item.value" @change="val => changeStatus(val, 'aaaaa')">
    <el-option v-for="op in options" 
               :key="op.key" 
               :label="op.label"
               :value="op.label"/>
  </el-select>
</div>
````

------


> + 日期选择器时间选择范围限制

````javascript

<el-date-picker
     v-model="value"
     type="date"
     placeholder="选择日期"
     :picker-options="datePickerOptions"
  ></el-date-picker>

data() {
    return {
      datePickerOptions: {
        disableDate(time) {
          //选择今天以及今天以前的日期  如果没有后面的-8.64e6就是不可以选择今天的
          return time.getTime() > Date.now() - 8.64e6;
        }
      }
    }
  }

//选择今天以及今天之后的日期  如果没有后面的-8.64e7就是不可以选择今天的 
return time.getTime() < Date.now() - 8.64e7;


//***********************


<el-date-picker
    v-model="startDate"
    type="date"
    placeholder="开始时间"
    :picker-options="datePickerOptions1"
></el-date-picker>

<el-date-picker
    v-model="endDate"
    type="date"
    placeholder="结束时间"
    :picker-options="datePickerOptions2"
></el-date-picker>

data() {
    return {
      datePickerOptions1: {
        disableDate(time) {
          if(this.endDate != "") {
            return time.getTime() > +new Date(this.endDate);
          }
        }
      },
      datePickerOptions2: {
        disableDate(time) {
          //减去一天的时间表示可以选择同一天
          return time.getTime() < +new Date(this.startDate) - 24*60*60*1000;
        }
      },
    }
  }

````

























