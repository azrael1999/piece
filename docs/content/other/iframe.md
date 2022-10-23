# iframe


````html
<div>
  <iframe id="son" src="a.html"></iframe>
</div>
````


> + 参数传递

event 的属性有：  
data: 从其他 window 传递过来的数据副本。   
origin: 调用 postMessage 时，消息发送窗口的 origin。例如：“http://example.com:8080”。  
source: 对发送消息的窗口对象的引用。可以使用此来在具有不同 origin 的两个窗口之间建立双向数据通信。   

````javascript
//子组件传父组件，*号代表所有源，涉及隐私要限制一下
window.parent.postMessage({isConfirm: true}, "*");

//父组件传子组件
let iframe1 = document.querySelector("#son");
iframe1.onload = () => {
  iframe1.contentWindow.postMessage(params, "*");
};

//打开新窗口
const targetWindow = window.open("http://localhost:10001/user");
targetWindow.onload = () => {
  targetWindow.postMessage(params, "*");
};

//***********************************

//接收的组件一般要过滤消息的来源
window.addEventListener("message", event => {
  let {data, origin, source} = event;
  if(origin !== "http://example.org:8080") {
    return;
  }
  let {isConfirm} = event.data;
  //do some things....
  //source是消息源，可以把某些消息传回去
  source.postMessage(params, "*");
});

````


> + 方法调用(不常用)
````javascript
 //当页面中有iframe时，想在主页面调用iframe中的方法，可以用contentWindow属性，但具体使用时还有一点要主要，就是必须等页面加载完成才可以，否则会报错找不到函数。  
 //sendMessage为子组件的方法,子组件可以获取到参数
  
   window.onload = function () {
     document.querySelector("#son").contentWindow.sendMessage(params);
   }
   //或者
   let iframe1 = document.querySelector("#son");
   iframe1.onload = function () {
     iframe1.contentWindow.sendMessage(params);
   }
 
//***********************************
 
//iframe子窗口调用父窗口方法

parent.funcName(params);

````






