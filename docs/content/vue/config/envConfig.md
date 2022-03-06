# .env

> + 说明

关于文件名：  
必须以如下方式命名，不要乱起名，也无需专门手动控制加载哪个文件    
`.env` 全局默认配置文件，不论什么环境都会加载合并    
`.env.development` 开发环境下多配置文件    
`.env.production` 生成环境下的配置文件    

关于文件内容：  
注意：属性名必须以`VUE_APP_`开头，比如`VUE_APP_abc`    
`process.env`属性(全局属性，任何地方均可以使用)    
````
console.log(process.env);

if(process.env === "dev") {}

````


> + 文件案例   
 
与`package.json`、`vue.config.js`同级目录

**.env**
````
NODE_ENV=development
VUE_APP_BUILD_TARGET=dev
VUE_APP_APPID_TARGET=dev
````


**.env.development**
````
NODE_ENV=development
VUE_APP_BUILD_TARGET=dev
VUE_APP_APPID_TARGET=dev
````

**.env.production**
````
NODE_ENV=production
VUE_APP_BUILD_TARGET=prod
VUE_APP_APPID_TARGET=prod
````


> + 进阶-通过env配置区分不同环境文件    

创建3个js文件，分别对应env的3个环境(一个是默认的，就当作是三个)，里头定义一些常量，可以是不同环境对应的api、参数，  
这些当然可以定义在不同多env配置里，但这个文件不是js，不好操作，属性名也难用，  
通过env环境的属性判断引入对应的文件，几个文件定义的常量都是同名的，使用的地方无需再判断  


创建config文件夹，与`package.json`、`vue.config.js`同级目录，几个文件都在里面    

**xxxx.dev.js** (xxxx是项目名，也可以随意)
````javascript
module.exports = {
  ihelp: {
    appId: "evaluation_service",
    env: "//nkweb-uat.huawei.com",
  },
  contextPath: "/",
  gateway: {
    "x-app-id": "com.huawei.ipd.alm.quality",
    "x-sub-app-id": "evaluation_service",
    contentx: "/evaluation-service",
    
    "x-multi-consumer-appid": "com.huawei.ipd.alm.quality",
    "x-multi-consumer-sub-appid": "common_service",
    "x-multi-consumer-context": "/common_service",
    
    services: {
      "x-app-id": "com.huawei.ipd.alm.quality",
      "x-sub-app-id": "evaluation_service",
      contentx: "/evaluation-service",
    },
    
    MasGatewayAddress: "//localhost.huawei.com:8003/quality-gateway/",
  },

  dtsServiceUrl: "https://ngdts.gamma.tools.huawei.com/DTSPortal/ticket/",
}
````

**xxxx.prod.js**
````javascript
module.exports = {
  ihelp: {
    appId: "evaluation_service",
    env: "//nkweb-uat.huawei.com",
  },
  contextPath: "/",
  gateway: {
    "x-app-id": "com.huawei.ipd.alm.quality",
    "x-sub-app-id": "evaluation_service_prod",
    contentx: "/evaluation-service_prod",
    
    "x-multi-consumer-appid": "com.huawei.ipd.alm.quality",
    "x-multi-consumer-sub-appid": "common_service",
    "x-multi-consumer-context": "/common_service",
    
    services: {
      "x-app-id": "com.huawei.ipd.alm.quality",
      "x-sub-app-id": "evaluation_service_prod",
      contentx: "/evaluation-service_prod",
    },
    
    MasGatewayAddress: "//rnd-eval-gateway.starling.huawei.com/eproject-quality-gateway/",
  },
 
  dtsServiceUrl: "https://ngdts.gamma.tools.huawei.com/DTSPortal/ticket/",
}
````

**这个文件是总的出口，这里面判断引入哪个文件**  
如果需要，这个文件还可以定义一些所有环境都一样的常量，或者从外部import，但以普遍理性而论，配置文件不应放这些东西
既然都是exports，另起一个文件跟符合规范    
**xxxx.js**
````javascript
let target = VUE_APP_BUILD_TARGET || "dev";
let config = require(`./xxxx.${target}`)

const tableAttr = {
  maxHeight: 530,
  minHeight: 240,
};
const t = "Destroy this world";

config = Object.assingn({}, config, {tableAttr, t}, {runTimeEnv: target});

module.exports = config;
````


使用配置文件的属性
````
<script>
import {tableAttr} from "@/../config/xxxx.js"
</script>
````





