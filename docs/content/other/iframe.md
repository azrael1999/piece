# iframe


````html
<iframe id="son" src="a.html"></iframe>
````

````javascript

 当页面中有iframe时，想在主页面调用iframe中的方法，可以用contentWindow属性，但具体使用时还有一点要主要，就是必须等页面加载完成才可以，否则会报错找不到函数。
   window.onload = function () {
     document.querySelector("#son").contentWindow.sendMessage();
   }
   //或者
   var iframe1 = document.querySelector("#son");
   iframe1.onload = function () {
     iframe1.contentWindow.sendMessage();
   }
 
   //***********************************
 
   iframe子窗口调用父窗口方法
   parent.funcName();


````






