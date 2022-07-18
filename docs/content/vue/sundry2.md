# vue杂项2

> + vue watch

````
watch: {
  customObj: {
    handler(newVal, oldVal) {
      console.log(newVal);
    },
    immediate: true,
    deep: true,
  },
  pbiCode(newVal) {
    console.log(newVal);
  },
}
````


> + provide inject

**特别注意**
父组件和子组件的`changeText`方法，都可以改变**父组件**的**text**值;  
子组件的**text**值不会更新，`inject`只会加载一次，要获取实时更新多值用`props`或者`vuex`;   
因此注入一般用来传一些常量、整个组件
````
<template>
  <div>
    <el-button @click="changeText">父组件按钮</el-button>
  </div>
</template>

<script>
  export default {
    name: "父组件",
    components: {},
    mixins: [],
    provide() {
      return {
        text: this.text,
        changeText: this.changeText,
        $parent: this,
      }
    },
    data() {
      return {
        text: "举杯邀明月",
      }
    },
    methods: {
      changeText() {
        this.text = "举杯邀明月，对影成三人";
      },
    },
    created() {},
    mounted() {},
    computed: {},
    watch: {},
  }
</script>

<style lang="less" scoped></style>
````

````
<template>
  <div>
    <el-button @click="changeText">子组件按钮</el-button>
    <p>{{text}}</p>
  </div>
</template>

<script>
  export default {
    name: "子组件",
    components: {},
    mixins: [],
    inject: ["text", "changeText", "$parent"],
    data() {
      return {}
    },
    methods: {},
    created() {},
    mounted() {},
    computed: {},
    watch: {},
  }
</script>

<style lang="less" scoped></style>
````


> + class 与 style 绑定

1. 数组，绑定每一项
2. 对象，绑定每个key，当value为truthy时  
在javescript中，truthy(`真值`)指的是在布尔值上下文中，转换后的值为真的值。
所有值都是`真值`，除非它们被定义为`假值`(即除 false、0、-0、0n、""、null、undefined 和 NaN 以外，皆为真值)。  

在数组语法中也可以嵌套对象语法，即数组每一项可以是字符串或者对象
````
<div class="=lights postion-r" @mouseleave="lightsLeave($event)" slot="reference">
      <span class="inline-block cursor-pointer position-a"
            :class="['light', 
            (neon.find(v => v.label === row.current) && neon.find(v => v.label === row.current).css || ''),
            {editable: !readOnly, aaaaa: true}]"
            @mouseenter="lightEnter($event)"
      />
      <ul class="switch position-a hidden">
        <li v-for="(item, index) in neon"
            :key="index"
            :class="['ligth', 'fl', 'mr-10', 'cursor-pointer', item.css]"
            :title="item.label || 'ReSet'"
            @click="turnOnLight(index)"
        />
      </ul>
    </div>
````

> + props传值

复杂类型默认值要函数返回值
````
props:{
      atomicType: {
        type: [String, Number],
        default: 1,
      },
      data: {
        tyep: Array,
        default: () => {
          return [];
        },
      },
      column: {
        tyep: Object,
        default: () => {
          return {};
        },
      },
    },
````




















