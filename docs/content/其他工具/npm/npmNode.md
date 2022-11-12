# npm 更新依赖包三种用法

> + 一、基本命令使用

* 查看全局安装的包
````
npm list -g --depth 0
````

* 查看安装包 `packageName` 最新发布的版本信息
````
npm view | info packageName version
````

* 查看远程安装包 `packageName` 的所有发布的版本信息
````
npm view | info packageName versions
````

* 检查过时的安装包
````
npm outdated [packageName]
````
![img](./6d73a3e099ea4b10a925c0721fd20f17_tplv-k3u1fbpfcp-watermark.awebp)


###### 版本信息说明
> Package 显示包名。若使用了 --long / -l 则还是显示这个包属于 dependencies 还是devDependency  
> Current  当前依赖包安装版本  
> Wanted 根据 package.json 包版本前缀规则可以更新的最新版本号  
> Latest 最新包版本号【默认情况下是最新的，这取决于开发人员的包管理制度】  
> Location 是该依赖包在所居于的依赖树中所在的位置  



###### package 字体颜色含义
> 红色 `package.json` 中包版本前缀规则可更新的依赖包  
> 黄色 `package.json` 中包版本前缀规则不可更新的依赖包   


##### 依赖版本认知
项目对应依赖包一般保存在 `package.json` 文件中，相对应版本号的形式为`mojor.minor.patch`
> major 表示非兼容的重大 API 改变（主要的）  
> minor 表示向后兼容的功能性改变（次要的）  
> patch 表示向后兼容的 bug 修正（修补的）  


###### 依赖包对应版本号前缀符号含义


> `*` 匹配最新的 `major` 版本依赖包<br/>
> `^` 匹配最新的 `minor` 版本依赖包，eg: `1.1.0` 可以更新匹配所有 `1.x.x` 的包，不会更新匹配  `2.x.x`   <br/>
> `~` 匹配最新的 `patch` 版本依赖包，eg: `1.1.0` 可以更新匹配所有 `1.1.x` 的包，不会更新匹配  `1.2.x`   <br/>
> 没有前缀表示固定版本号, 版本不会更新匹配任何其他版本。【需要手动修改 `package.json` 包版本】   <br/>


> + 二、npm update

更新指定依赖安装包【不一定包括 `major` 位的更新，有时需要在 `package.json` 手动更改依赖包相应版本号在更新】
> -S `dependencies` 生产环境下依赖安装(--save)，默认安装  
> -D `devDependencies` 开发环境下依赖安装(--save-dev)  
````
npm update packageName (-D | -S)
````


###### npm i 与npm update之间的区别
下面描述：
* `package` 表示 package.json 依赖版本管理文件
* `lock` 表示 package-lock.json 锁定依赖版本文件


1. `lock` 文件存在
> * `npm i` 会按照 `lock` 对应包版本进行安装，不会自动升级  
> > 手动更改 `package` 中对应包, `lock` 会按照 `package` 前缀版本规范更新到最新版本, `package` 版本为手动版本不变    
> * `npm update` 会按照 `package` 对应包版本前缀升级规范安装到最新到版  
> > `package` 中对应包版本号改变【按前缀规范最新版与 `lock` 中相同则不变，不同则改变】  
> > `lock` 中对应包版本号改变  


2. `lock` 文件不存在

> * `npm i` 会按照 `package` 对应包版本前缀升级规范安装到最新到版本  
> > `package` 仍然是之前的前缀规范版本号    
> > `lock` 中按版本前缀规范升级到最新的版本  
> * npm update 和 npm i 相似   
> > 但会忽略 devDependencies 下的对应包更新安装     
> > 添加了 -D 才会在安装更新 dependencies 下的前提下更新安装 devDependencies 下对应的依赖包  



> + 三、npm-check-updates

* 全局安装依赖 npm-check-update
````
npm install npm-check-update -g
````

* 检查可更新模块
````
ncu
或
npm-check-update
````

* 更新可更新模块【并不建议一次性更新所有可更新依赖包】（更新包括 major 位的更新）
````
ncu -u [packageName]
````


> + 四、npm-check

* 全局安装依赖  npm-check
````
npm install npm-check -g
````

* 查看可更新包信息
````
npm-check
````

* 选择并更新相应的依赖包【空格选择、enter更新】
````javascript
npm-check -u
````

> + 五、三种方法的区别
##### 区别:

###### npm update
1. `npm update [packageName]` 会同步更新 `package-lock.json` 文件中对应的包的版本，不需要重新安装 npm 包

###### npm-check-updates 和 npm-check
1. 二者基本大致相同，只是在更新过程中的一些交互表现形式存在一定的差异
2. 更新 `package.json` 文件中可更新的安装包，但不会更新对应的 `package-lock.json` 文件中对应的包的版本
3. 需要使用下面命令重新安装依赖：
````
rm -rf package-lock.json && npm i
````

