
# npm link的用法

假如我们想自己开发一个依赖包，以便在多个项目中使用。  
一种可行的方法，也是npm给我们提供的标准做法，那就是我们独立开发好这个依赖包，然后将它直接发布到npm镜像站上去，等以后想在其他项目中使用的时候，直接npm install moduleName。   
但是，如果我们修改了这个依赖包的源码，就要重新发布到npm镜像站，这样做相对来说会有一点麻烦。  
我们希望有更方便一点的办法，npm link就是这样的一个简便方案。    


![img](./54308.png)



> + commonModule
在`commonModule`下运行`npm init`命令，这会在`commonModule`下生成`package.json`文件。  
 
![img](./161136.png)

````
// methods.js
function print(text) {
  console.log("print--  ", text);
}
export {
  print
}
````

````
// inex.js
export * from "./tool/methods";
````



> + projectTwo

在项目脚本里加入npm link 路径，node_modules下会生成一个包  
需要注意的的是，`packageName`是取自包的公共模块的`package.json`中`name`字段，不是文件夹名称  
**import的时候要注意**
  
![img](./161816.png)

````
<template>
  <div>
    <p>嘿嘿嘿</p>
  </div>
</template>

<script>
  import  { print } from "commonmodule";
  export default {
    methods: {
      print,
    },
    created() {
      this.print("哈哈哈");
    },
  }
</script>

<style lang="less" scoped></style>
````



  