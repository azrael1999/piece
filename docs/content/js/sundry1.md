# js杂项1

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


















