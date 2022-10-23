> + 某些包使用此处配置的源


package.json同目录下   .npmrc 文件  
继承C盘下的全局配置,要改某个包的源：  包名 + 冒号 + 源  
不想修改全局源，只是当前项目要改源，也可以修改此处，好处还有此文件在git代码仓下，换电脑也不需要设置源
````
sass_binary_site=http://mirrors.tools.huawwe.com/node-sass/
@aurora:registry=http//:w3-beta.huawwe.com/artifactory/api/npm/npm-all/

unsafe-perm=true

````





