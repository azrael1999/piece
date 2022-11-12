# js杂项

> + addEventListener与removeEventListener

通过addEventListener添加的事件处理程序只能使用removeEventListener来移除；移除时传入的参数与添加处理程序时使用的参数相同  
通过addEventListener添加的匿名函数无法移除,因为两个方法并不相等，内存地址已经是不同的
````
methods: {
  closeMessage() {
    this.message && this.message.close();
  },
},
mounted() {
  window.addEventListener("click", this.closeMessage);
},
beforeDestroy() {
  window.removeEventListener("click", this.closeMessage);
},
````



> + 从标签中获取innerText

````
new DOMParser().parseFromString(template, "text/html").querySelector("body").innerText;

// template 是带标签的字符串
// new DOMParser().parseFromString(template, "text/html") 可以转换为Dom元素

````



> + 属性检查

hasOwnProperty
这个方法可以用来检测一个对象是否含有特定的自身属性；和 in 运算符不同，该方法会忽略掉那些从原型链上继承到的属性。
原型链上继承到的属性返回的是 false
````javascript
var customObj = {
  a: "aa",
  b: "bb",
};

customObj.hasOwnProperty("a");
Object.prototype.hasOwnProperty.call(customObj, "a");  //适合用在封装的函数内部,对象和属性都是传进来的

````



> + e.target与e.currentTarget的区别
````javascript
  e.target 指向触发事件监听的对象
  
  e.currentTarget 指向添加监听事件的对象，vue里是绑定里事件的元素，可以利用事件冒泡的机制绑定在触发事件元素的父元素上，比如ul。
````















