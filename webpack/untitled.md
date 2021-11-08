# 入门

> webpack 是一个模块化打包工具。webpack 主要是用来开发应用程序。而 rollup 主要是用来开发 library 库(因为 rollup 可以一套代码，构建成 UMD,AMD,CJS,ESM,IIFE 等模式，更适合做库)

webpack 几个重要特征\
1.input 入口\
2.output 出口\
3.loader 将不同的文件类型转换为 webpack 能识别的模块(个人认为是最重要的概念)\
4.plugins 插件(扩展能力)

## 版本区分

截止2021.9.2，webpack的最新版本latest已经切换到5.x。所以想安装4.x必须手动安装一下。

```
npm i webpack@4.46.0 webpack-cli@3.3.12 -D    两个对应的适配版本,适用于webpack4
npm i webpack webpack-cli -D    默认安装最新的latest适配版本，当前就是webpack5
```

### &#x20;快速上手

推荐直接查看 [指南](https://www.webpackjs.com/guides/getting-started/#%E4%BD%BF%E7%94%A8%E4%B8%80%E4%B8%AA%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6) 快速上手

总结一些常用的:

```
//webpack.config.js
const path = require("path");
const { VueLoaderPlugin } = require("vue-loader");
const { HotModuleReplacementPlugin, DefinePlugin } = require("webpack");
const { BundleAnalyzerPlugin } = require("webpack-bundle-analyzer");
module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  devServer: {
    contentBase: "./dist", //开发服务器所在目录，将index.html放入，是为了默认访问index.html，dev-server模式下，不会产出实际的bundle.js，但是仍可引用，并live reloading代码(注意:这里不会通知浏览器，仍然需要你手动刷新)
    hot: true,
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        use: "vue-loader",
      },
      {
        test: /\.txt$/,
        use: "raw-loader",
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        use: {
          loader: "url-loader",
          options: {
            limit: false,
            // esModule:false //注意开关
          },
        },
      },
      {
        test: /\.m?js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"], //bable转换
          },
        },
      },

      {
        test: /\.css$/,
        use: ["style-loader", "vue-style-loader", "css-loader"],
      },
    ],
  },
  plugins: [
    // new BundleAnalyzerPlugin(), //分析插件
    new HotModuleReplacementPlugin(),
    new VueLoaderPlugin(),
    new DefinePlugin({
      "process.env.NODE_ENV": JSON.stringify("可以直接改写这个，但是在vue-cli中是不允许的"),
      beijjing: JSON.stringify("北京"),
    }),
  ],
  externals: {
    vue: "Vue", //排除依赖
  },
  resolve: {
    alias: {
      //别名配置
      "@": path.resolve(__dirname, "src"),
      "@assets": path.resolve(__dirname, "src/assets"),
    },
  },
};

```

### 实用插件

#### npm webpack-dev-server@3.11.2 -D&#x20;

每次写完都要静态打包，很麻烦。起个服务，让代码发生变化后自动编译代码(编译到内存中),虽然走的是 output 配置输出，但是不会编译出实际的 bundle.js，不过仍然可以引用。 HMR 在 webpack 内置模块中配置插件 new webpack.HotModuleReplacementPlugin()

#### npm i raw-loader@4.0.2 -D&#x20;

可以用来加载.txt 文件，加载进去的是纯文本

#### npm i url-loader@4.1.1 file-loader@6.2.0 -D&#x20;

url-loader 和 file-loader。 url-loader 可以限制文件大小，转为 DataURL。默认 8192。\
file-loader 克隆资源： 将文件发送到输出文件夹，并返回（相对）URL。 两个都要安装，但是配置只需要配置url-loader就行&#x20;

版本差异: 在file-loader 5.0.0之后，已将esModule=true设置为默认值,在vue中src引入图片时，建议采用require('../image.png').default形式，否则会src="\[object Module]" 找不到图片，且不能直接 src="../iamge.png，因为这种形式会转化为requre()但是没有default。或者在url-loader配置中手动关闭即可

#### npm i babel-loader@8.2.2 @babel/preset-env@7.15.4 @babel/core@7.15.4 -D&#x20;

babel 非常重要的 vue-cli 将 babel-loader @babel/preset-env(预设环境，很重要的一个) @babel/core 三个包合在了一起，叫 @vue/cli-plugin-babel 注意:需要有 .browserslistrc 加持，这样可以自动检查需要转换的版本

#### npm i vue-loader@15.9.8 vue-template-compiler@2.6.11

解析.vue 文件 需要安装 vue-loader 和 vue-template-compiler 注意vue-template-comiler需要和实际的vue版本保持一致 [https://vue-loader.vuejs.org/zh/guide/#手动设置](https://vue-loader.vuejs.org/zh/guide/#%E6%89%8B%E5%8A%A8%E8%AE%BE%E7%BD%AE) , 单SFC特性可以去vue-loader文档查看

#### npm i webpack-bundle-analyzer@4.4.2 -D&#x20;

依赖可视化分析。vue-cli有自带的vue ui 更强大

#### webpack.DefinePlugin

内置插件。可以指定环境，可以使用node中的process.env.NODE\_ENV==='production'判断

### webpack内置配置

> 排除依赖。webpack内置配置 externals: { vue: "Vue", //key 是 package.json 中的依赖名，value 是实际使用的变量 }。文档

> 别名配置 resolve.alias中配置即可 [https://cli.vuejs.org/zh/guide/html-and-static-assets.html#url-转换规则](https://cli.vuejs.org/zh/guide/html-and-static-assets.html#url-%E8%BD%AC%E6%8D%A2%E8%A7%84%E5%88%99) 别名@只在模板中，js引入中可以使用， 在css中是无法使用的。 如果使用 \~开头，后面的内容可以当模块解析

> 是否需要压缩输出。webpack 可根据 mode 自动采用是否压缩。默认 production 为压缩，可手动更改为 mode:"development"，这样打包出来的代码将不会压缩。 开发模式不需要压缩，开发模式下我们需要更多的 sourcemap，更精准的定位，热模块替换。 生产模式需要压缩，生产模式只关注更小更轻的 bundle 和 sourcemap

> 关于 source map
>
> devtool 属性,如果不配置的情况下。就是压缩代码，所有代码在一起，
>
> 开发模式:我们一般希望更加精准的源码定位，所以需要牺牲速度。增加 bundle 体积.推荐 source-map 就可 生产模式:分两种:1.完全舍弃源码映射，只要打包之后的 bundle 就行。 2.源码映射与 bundle 分别打包，部署 bundle,源码映射单独部署(可做错误信息监测)。或者限制普通用户访问 sourcemap

> 1. 打包后的代码。也就是不配置。那么完全没有 sourcemap 映射。(一般标准生产)
> 2. 生成后的代码。（eval）都在一个文件中，不能正确显示行数
> 3. 转换过的代码。（无法看，不利于定于，一般用于第三方库）
> 4.  原始源代码。利于精准定位
>
>     3.1 eval-source-map (在 bundle 中采用将 sourcemap 转换为 DataUrl 后添加到 eval 中) build 慢 rebuild 快
>
>     3.2 source-map 生成外部.map 文件(源码级别) 单独的文件，更加灵活
>
> eval 虽然代码都打在 bundle 中，但是单个模块拥有区分，可被浏览器解析 sourcemap。查看形式变成 入口.js+n\_modules eval-source-map 浏览器解析时，可以解析出对应的源码文件。 每个模块相互分离，就像你编写那样。 sourcemap 文档地址

## vue-cli 配置环境变量，配置 html-webpack-plugin。

关于 vue-cli 的配置。可以使用 vue inspect > output.js 打印所有的配置

### 1.环境变量

vue-cli 的环境变量默认有三种形式\['development','production','test'],使用 proceess.env.NODE\_ENV 可以取到

* vue-cli-service serve 起开发服务 为 development
* vue-cli-service build 打包 为 prodction
* vue-cli-service test:unit 测试为 test

但是我们的实际情况不止，比如打包后还需要区分部署测试环境与生产环境。测试环境需要有调试面板,生产环境需要使用 cdn，排除依赖等。

例子: 你需要在某个测试环境中使用调试面板.暂定这个自定义的模式为 uat

```javascript
//  ---script启动脚本增加参数---
"scripts": {
    "build:uat": "vue-cli-service build --mode uat",
}
```

// ------此时启动模式为 uat,会自动加载.env.uat 文件,参考下面的 mode 加载配置文件文档 .env.uat 文件

> 使用自定义 mode 后，proceess.env.NODE\_ENV 默认为 development，根据自身需求决定是否重写
>
> 自定义的变量必须要使用 VUE_APP_开头.

```
# npm run build:uat 就走这个配置

# uat打包，需要手动注入NODE_ENV为'production',否则为'development'
NODE_ENV=production

# 当前环境的名称 自定义的变量,这样就可以实现在项目中自由使用了
VUE_APP_MODE=uat
```

[mode 加载配置文件文档](https://cli.vuejs.org/zh/guide/mode-and-env.html#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)

[https://mp.weixin.qq.com/s/UrIH72bYufUxCoXs54QqlQ](https://mp.weixin.qq.com/s/UrIH72bYufUxCoXs54QqlQ)
