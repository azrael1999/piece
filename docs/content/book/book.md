# book说明


目前用的是`docsify`，这个比`Gitbook`和`VuePress`简单，不过插件会少一点



> + 安装

首先全局安装 `docsify-cli`：

```
npm i docsify-cli -g
```


> + 初始化

假设我们要在 `./docs` 子目录中编写文档，将该目录初始化：

```
docsify init ./docs
```

初始化后系统帮我们生成了一个 `./docs` 目录，目录中包含以下文件：

- `index.html`：入口文件
- `README.md`：将作为主页渲染
- `.nojekyll`：阻止 Github Pages 忽略以下划线开头的文件


> + 预览

使用以下命令启动本地服务器：

```
#在index.html目录下运行
docsify serve

#or 在上层或别的目录下运行
docsify serve [目录]
```





