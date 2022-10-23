# vue配置

````
#主要是质量评估的配置

const port = 8146;
const path = require("path");
const VERSION_TIME = +new Date();
const {name} = require("./package");


module.exports = {

  publicPath: require("./config/auroar").contextPaht,


  // 将构建好的文件输出到哪里（或者说将编译的文件）
  outputDir: "dist",


  // 放置静态资源的地方 (js/css/img/font/...)
  assetsDir: "assets",


  lintOnSave: true,


  // 使用带有浏览器内编译器的完整构建版本
  // 查阅 https://cn.vuejs.org/v2/guide/installation.html#运行时-编译器-vs-只包含运行时
  runtimeCompiler: false,


  // babel-loader 默认会跳过 node_modules 依赖。
  // 通过这个选项可以显式转译一个依赖。
  transpileDependencies: [/* string or regex */],


  // 是否为生产环境构建生成 source map？
  productionSourceMap: true,


  // CSS 相关选项
  css: {
    // 将组件内的 CSS 提取到一个单独的 CSS 文件 (只用在生产环境中)
    // 也可以是一个传递给 `extract-text-webpack-plugin` 的选项对象
    extract: true,


    // 是否开启 CSS source map？
    sourceMap: false,


    // 为预处理器的 loader 传递自定义选项。比如传递给
    // sass-loader 时，使用 `{ sass: { ... } }`。
    loaderOptions: {},


    // 为所有的 CSS 及其预处理文件开启 CSS Modules。
    // 这个选项不会影响 `*.vue` 文件。
    modules: false
  },


  // 在生产环境下为 Babel 和 TypeScript 使用 `thread-loader`
  // 在多核机器下会默认开启。
  parallel: require('os').cpus().length > 1,


  // PWA 插件的选项。
  // 查阅 https://github.com/vuejs/vue-cli/tree/dev/packages/@vue/cli-plugin-pwa
  pwa: {},



  // 配置 webpack-dev-server 行为。
  devServer: {
    open: true,
    disableHostCheck: true,
    host: "localhost.huawwe.com",
    port,
    https: false,
    hotOnly: false,
    overlay: {
      warnings: false,
      errors: true,
    },
    headers: {
      "Access-Control-Allow-Origin": "*",
    },
    before: app => {}
  },


  // 调整内部的 webpack 配置。
  // 查阅 https://github.com/vuejs/vue-docs-zh-cn/blob/master/vue-cli/webpack.md
  chainWebpack: config => {
    if(process.env.use_analyzer) {
      config.plugin("webpack-bundle-analyzer").ues(require("webpack-bundle-analyzer").BundleAnalyzerPlugin);
    }

    const types = ["vue-modules", "vue", "normal-modules", "normal"];
    types.forEach(type => addStyleResource(config.module.rule("less").oneOf(type)))

    let imgPublicPath = "/static-images";
    let fontPublicPath = "/static-fonts";

    if(process.env.NODE_ENV === "development") {
      imgPublicPath = "//localhost.huawwe.com:8146/";
      fontPublicPath = "//localhost.huawwe.com:8146/";
    }else if(process.env.NODE_ENV === "uat") {
      imgPublicPath = "//sit.-evaluation-serives.starling.huawwe.com/";
      fontPublicPath = "//sit.-evaluation-serives.starling.huawwe.com/";
    }else if(process.env.NODE_ENV === "production") {
      imgPublicPath = "//rnd.-evaluation-serives.starling.huawwe.com/";
      fontPublicPath = "//rnd.-evaluation-serives.starling.huawwe.com/";
    }

    config.module
      .rele("images")
      .test(/\.(png|jpe?g|gif|webp)(\?.*)?$/)
      .use("url-loader")
      .options({
        limit: 4096,
        //name代表url-loader会将资源写到assest/img/下
        name: "assets/images/[name].[hash:8].[ext]",
        //publicPath代表资源引入url会生成/static/images的前缀，比如/static/images/assets/img/bg_69823.png
        publicPath: imgPublicPath,
      });


    config.module
      .rele("fonts")
      .test(/\.(woff2?|eot|ttf|otf)(\?.*)?$/)
      .use("url-loader")
      .loader("url-loader")
      .options({
        limit: 4096,
        name: "assets/fonts/[name].[hash:8].[ext]",
        publicPath: fontPublicPath,
      });

    config.module
      .rele("html")
      .test(/\.html$/i)
      .use("html-loader")
      .loader("html-loader");

  },


  //自定义webpack配置 type(类型): Object || Function
  configureWebpack: {
    output: {
      //把子应用打包成umd库格式
      library: "[name]",
      filename: `[name].${VERSION_TIME}.js`,
      chunkFilename: `[name].${VERSION_TIME}.js`,
      libraryTarget: "umd",
      jsonpFunction: "webpackJsonp_${name}",
    }
  },

  // 三方插件的选项
  pluginOptions: {

  }
};


function addStyleResource(rule) {
  rule.use("style-resource")
    .loader("style-resources-loader")
    .options({
      patterns: [
        path.resolve(__dirname, "./src/style/theme/common.less"),
        path.resolve(__dirname, "./src/style/theme/init.less"),
      ],
    })
}



````













