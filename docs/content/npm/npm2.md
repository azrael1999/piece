> + 查看本地插件和版本

````
#ls是list的缩写，depth参数后有 0、1。1是展开详细

#查看本机安装的插件，任意目录，第二条查看本地全局安装的某个插件
npm list -g –depth 0
npm ls jquery -g


#查看项目安装的插件，在项目目录下，node_modules的同级目录
npm list


#查看项目安装的某个插件版本，node_modules的同级目录
npm list jquery
npm ls jquery

````



> + 查看npm仓库插件版本

````
#第一种方式：使用npm view jquery versions

#这种方式可以查看npm服务器上所有的jquery版本信息；

                    

#第二种方式：使用npm view jquery version

#这种方式只能查看jquery的最新的版本是哪一个；

                    

#第三种方式：使用npm info jquery

#这种方式和第一种类似，也可以查看jquery所有的版本，但是能查出更多的关于jquery的信息；

                    

````


