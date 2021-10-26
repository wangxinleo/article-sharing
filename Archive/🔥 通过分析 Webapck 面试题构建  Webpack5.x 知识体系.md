> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [juejin.cn](https://juejin.cn/post/7023242274876162084?utm_source=gold_browser_extension)*   没有更多实践的机会，往往会让人忽视对它的学习
*   想学习的时候，又不知道从何下手🤦‍♂️

这个时候，我们可以先看通常在面试时候，关于 Webpack 都会问些什么

**为什么要从面试题下手？**

1.  了解面试官具体希望候选人具备哪些 Webpack 相关知识
2.  方便我们捕捉学习 Webpack 的知识重点

这里我找了一些代表性的问题，试试看你能回答多少 😉

> 问：Webpack 配置中用过哪些 Loader ？都有什么作用？  

> 问：Webpack 配置中用过哪些 Plugin ？都有什么作用？  

> 问：Loader 和 Plugin 有什么区别？  

> 问：如何编写 Loader ? 介绍一下思路？  

> 问：如何编写 Plugin ? 介绍一下思路？  

> 问：Webpack optimize 有配置过吗？可以简单说说吗？  

> 问：Webpack 层面如何性能优化？  

> 问：Webpack 打包流程是怎样的？  

> 问：tree-shaking 实现原理是怎样的？  

> 问：Webpack 热更新（HMR）是如何实现？  

> 问：Webpack 打包中 Babel 插件是如何工作的？  

> 问：Webpack 和 Rollup 有什么相同点与不同点？  

> 问：Webpack5 更新了哪些新特性？  

怎么样？这些问题都 OK 吗？

接下来，我们拆解归纳一下这些面试题，绘制一个大概的知识体系

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4588ccf24c024448847ecfe6eb8722b0~tplv-k3u1fbpfcp-watermark.awebp)

建立好知识体系之后，我们会围绕知识体系，简单分为三个层级：

1.  **基础** -- 会配置
2.  **进阶** -- 能优化
3.  **深入** -- 懂原理

一层层晋级打怪 🦖 ，解答不同深度的 Webpack 面试题问题 🤓

来，我们现在开始吧 🤜

一、Webpack 基础
------------

第一部分，先从简单的 “会配置” 的要求出发，先了解 Webpack 简单配置以及简单配置会涉及到的面试题。

### 1. 简单配置

> 该部分需要掌握：
> 
> 1.  Webpack 常规配置项有哪些？
> 2.  常用 Loader 有哪些？如何配置？
> 3.  常用插件（Plugin）有哪些？如何的配置？
> 4.  Babel 的如何配置？Babel 插件如何使用？

#### 1.1 安装依赖

毫无疑问，先本地安装一下 `webpack` 以及 `webpack-cli`

```
$ npm install webpack webpack-cli -D # 安装到本地依赖
复制代码

```

安装完成 ✅

```
+ webpack-cli@4.7.2
+ webpack@5.44.0
复制代码

```

#### 1.2 工作模式

webpack 在 4 以后就支持 0 配置打包，我们可以测试一下

1.  新建 `./src/index.js` 文件，写一段简单的代码

```
const a = 'Hello ITEM'
console.log(a)
module.exports = a;
复制代码

```

此时目录结构

```
webpack_work                  
├─ src                
│  └─ index.js         
└─ package.json       
复制代码

```

2.  直接运行 `npx webpack`，启动打包

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64c6c6a172d549959c57a936dded4c43~tplv-k3u1fbpfcp-watermark.awebp)

打包完成，我们看到日志上面有一段提示：`The 'mode' option has not been set,...`

意思就是，我们没有配置 mode（模式），这里提醒我们配置一下

> **模式：** 供 mode 配置选项，告知 webpack 使用相应模式的内置优化，默认值为 `production`，另外还有 `development`、`none`，他们的区别如下

<table><thead><tr><th>选项</th><th>描述</th></tr></thead><tbody><tr><td>development</td><td>开发模式，打包更加快速，省了代码优化步骤</td></tr><tr><td>producttion</td><td>生产模式，打包比较慢，会开启 tree-shaking 和 压缩代码</td></tr><tr><td>none</td><td>不使用任何默认优化选项</td></tr></tbody></table>

怎么配置呢？很简单

1.  只需在配置对象中提供 mode 选项：

```
module.exports = {
  mode: 'development',
};
复制代码

```

2.  从 CLI 参数中传递：

```
$ webpack --mode=development
复制代码

```

#### 1.3 配置文件

虽然有 0 配置打包，但是实际工作中，我们还是需要使用配置文件的方式，来满足不同项目的需求

1.  根路径下新建一个配置文件 `webpack.config.js`
    
2.  新增基本配置信息
    

```
const path = require('path')

module.exports = {
  mode: 'development', // 模式
  entry: './src/index.js', // 打包入口地址
  output: {
    filename: 'bundle.js', // 输出文件名
    path: path.join(__dirname, 'dist') // 输出文件目录
  }
}
复制代码

```

这个就不过多说了，最基本的配置

#### 1.4 Loader

这里我们把入口改成 CSS 文件，可能打包结果会如何

1. 新增 `./src/main.css`

```
body {
  margin: 0 auto;
  padding: 0 20px;
  max-width: 800px;
  background: #f4f8fb;
}
复制代码

```

2.  修改 entry 配置

```
const path = require('path')

module.exports = {
  mode: 'development', // 模式
  entry: './src/main.css', // 打包入口地址
  output: {
    filename: 'bundle.css', // 输出文件名
    path: path.join(__dirname, 'dist') // 输出文件目录
  }
}
复制代码

```

3. 运行打包命令：`npx webpack`

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db44786adb79445bb26bb86d37dff920~tplv-k3u1fbpfcp-watermark.awebp)

这里就报错了！

这是因为：**webpack 默认支持处理 js 文件，其他类型都处理不了，这里必须借助 Loader 来对不同类型的文件的进行处理。**

4.  安装 `css-loader` 来处理 CSS

```
npn install css-loader -D
复制代码

```

5.  配置资源加载模块

```
const path = require('path')

module.exports = {
  mode: 'development', // 模式
  entry: './src/main.css', // 打包入口地址
  output: {
    filename: 'bundle.css', // 输出文件名
    path: path.join(__dirname, 'dist') // 输出文件目录
  },
  module: { 
    rules: [ // 转换规则
      {
        test: /.css$/, //匹配所有的 css 文件
        use: 'css-loader' // use: 对应的 Loader 名称
      }
    ]
  }
}
复制代码

```

6.  重新运行打包命令 `npx webpack`

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/182ddecce9fa4dc08b7d55ab48dfd760~tplv-k3u1fbpfcp-watermark.awebp)

哎嘿，可以打包了 😁

```
dist           
└─ bundle.css  # 打包得到的结果
复制代码

```

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/026b10e416d84d1e87e2dab32e52e862~tplv-k3u1fbpfcp-watermark.awebp)

这里这是尝试，入口文件还是需要改回 `./src/index.js`

这里我们可以得到一个结论：**Loader 就是将 Webpack 不认识的内容转化为认识的内容**

#### 1.5 插件（plugin）

与 Loader 用于转换特定类型的文件不同，**插件（Plugin）可以贯穿 Webpack 打包的生命周期，执行不同的任务**

下面来看一个使用的列子：

1. 新建 `./src/index.html` 文件

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta >
  <title>ITEM</title>
</head>
<body>
  
</body>
</html>
复制代码

```

如果我想打包后的资源文件，例如：js 或者 css 文件可以自动引入到 Html 中，就需要使用插件 [html-webpack-plugin](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fhtml-webpack-plugin "https://www.npmjs.com/package/html-webpack-plugin") 来帮助你完成这个操作

2. 本地安装 `html-webpack-plugin`

```
npm install html-webpack-plugin -D
复制代码

```

3. 配置插件

```
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  mode: 'development', // 模式
  entry: './src/index.js', // 打包入口地址
  output: {
    filename: 'bundle.js', // 输出文件名
    path: path.join(__dirname, 'dist') // 输出文件目录
  },
  module: { 
    rules: [
      {
        test: /.css$/, //匹配所有的 css 文件
        use: 'css-loader' // use: 对应的 Loader 名称
      }
    ]
  },
  plugins:[ // 配置插件
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]
}
复制代码

```

运行一下打包，打开 dist 目录下生成的 index.html 文件

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta >
  <title>ITEM</title>
<script defer src="bundle.js"></script></head>
<body>
  
</body>
</html>
复制代码

```

可以看到它自动的引入了打包好的 bundle.js ，非常方便实用

#### 1.6 自动清空打包目录

每次打包的时候，打包目录都会遗留上次打包的文件，为了保持打包目录的纯净，我们需要在打包前将打包目录清空

这里我们可以使用插件 [clean-webpack-plugin](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fclean-webpack-plugin "https://www.npmjs.com/package/clean-webpack-plugin") 来实现

1.  安装

```
$ npm install clean-webpack-plugin -D
复制代码

```

2.  配置

```
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 引入插件
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
  // ...
  plugins:[ // 配置插件
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    new CleanWebpackPlugin() // 引入插件
  ]
}
复制代码

```

#### 1.7 区分环境

本地开发和部署线上，肯定是有不同的需求

**本地环境：**

*   需要更快的构建速度
*   需要打印 debug 信息
*   需要 live reload 或 hot reload 功能
*   需要 sourcemap 方便定位问题
*   ...

**生产环境：**

*   需要更小的包体积，代码压缩 + tree-shaking
*   需要进行代码分割
*   需要压缩图片体积
*   ...

针对不同的需求，首先要做的就是做好环境的区分

1.  本地安装 cross-env [[文档地址](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fcross-env "https://www.npmjs.com/package/cross-env")]

```
npm install cross-env -D
复制代码

```

2.  配置启动命令

打开 `./package.json`

```
"scripts": {
    "dev": "cross-env NODE_ENV=dev webpack serve --mode development", 
    "test": "cross-env NODE_ENV=test webpack --mode production",
    "build": "cross-env NODE_ENV=prod webpack --mode production"
  },
复制代码

```

3.  在 Webpack 配置文件中获取环境变量

```
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

console.log('process.env.NODE_ENV=', process.env.NODE_ENV) // 打印环境变量

const config = {
  entry: './src/index.js', // 打包入口地址
  output: {
    filename: 'bundle.js', // 输出文件名
    path: path.join(__dirname, 'dist') // 输出文件目录
  },
  module: { 
    rules: [
      {
        test: /.css$/, //匹配所有的 css 文件
        use: 'css-loader' // use: 对应的 Loader 名称
      }
    ]
  },
  plugins:[ // 配置插件
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]
}

module.exports = (env, argv) => {
  console.log('argv.mode=',argv.mode) // 打印 mode(模式) 值
  // 这里可以通过不同的模式修改 config 配置
  return config;
}
复制代码

```

4. 测试一下看看

*   执行 `npm run build`

```
process.env.NODE_ENV= prod
argv.mode= production
复制代码

```

*   执行 `npm run test`

```
process.env.NODE_ENV= test
argv.mode= production
复制代码

```

*   执行 `npm run dev`

```
process.env.NODE_ENV= dev
argv.mode= development
复制代码

```

这样我们就可以不同的环境来动态修改 Webpack 的配置

#### 1.8 启动 devServer

1. 安装 [webpack-dev-server](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fconfiguration%2Fdev-server%2F%23devserver "https://webpack.docschina.org/configuration/dev-server/#devserver")

```
npm intall webpack-dev-server -D
复制代码

```

2. 配置本地服务

```
// webpack.config.js
const config = {
  // ...
  devServer: {
    contentBase: path.resolve(__dirname, 'public'), // 静态文件目录
    compress: true, //是否启动压缩 gzip
    port: 8080, // 端口号
    // open:true  // 是否自动打开浏览器
  },
 // ...
}
module.exports = (env, argv) => {
  console.log('argv.mode=',argv.mode) // 打印 mode(模式) 值
  // 这里可以通过不同的模式修改 config 配置
  return config;
}
复制代码

```

**为什么要配置 contentBase ？**

因为 webpack 在进行打包的时候，对静态文件的处理，例如图片，都是直接 copy 到 dist 目录下面。但是对于本地开发来说，这个过程太费时，也没有必要，所以在设置 contentBase 之后，就直接到对应的静态目录下面去读取文件，而不需对文件做任何移动，节省了时间和性能开销。

3.  启动本地服务

```
$ npm run dev
复制代码

```

为了看到效果，我在 html 中添加了一段文字，并在 public 下面放入了一张图片 logo.png

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta >
  <title>ITEM</title>
</head>
<body>
  <p>ITEM</p>
</body>
</html>
复制代码

```

```
public       
└─ logo.png  
复制代码

```

打开地址 `http://localhost:8080/`

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/86b5b3175a2a47248c8da77cf51b752d~tplv-k3u1fbpfcp-watermark.awebp)

接着访问 `http://localhost:8080/logo.png`

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa86c70642db455c91a2c49129dfe9c2~tplv-k3u1fbpfcp-watermark.awebp)

OK，没问题 👌

#### 1.9 引入 CSS

上面，我们在 Loader 里面讲到了使用 css-loader 来处理 css，但是单靠 css-loader 是没有办法将样式加载到页面上。这个时候，我们需要再安装一个 style-loader 来完成这个功能

style-loader 就是将处理好的 css 通过 style 标签的形式添加到页面上

1.  安装 `style-loader` [[文档地址](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fstyle-loader "https://www.npmjs.com/package/style-loader")]

```
npm install style-loader -D
复制代码

```

2.  配置 Loader

```
const config = {
  // ...
  module: { 
    rules: [
      {
        test: /.css$/, //匹配所有的 css 文件
        use: ['style-loader','css-loader']
      }
    ]
  },
  // ...
}
复制代码

```

> **⚠️注意：** Loader 的执行顺序是固定从后往前，即按 `css-loader --> style-loader` 的顺序执行

3.  引用样式文件

在入口文件 `./src/index.js` 引入样式文件 `./src/main.css`

```
// ./src/index.js

import './main.css';


const a = 'Hello ITEM'
console.log(a)
module.exports = a;
复制代码

```

```
/* ./src/main.css */ 
body {
  margin: 10px auto;
  background: cyan;
  max-width: 800px;
}
复制代码

```

3.  重启一下本地服务，访问 `http://localhost:8080/`

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0a39e673799d42338feb714bc0d5c88e~tplv-k3u1fbpfcp-watermark.awebp)

这样样式就起作用了，继续修改一下样式

```
body {
  margin: 10px auto;
  background: cyan;
  max-width: 800px;
  /* 新增 */
  font-size: 46px;
  font-weight: 600;
  color: white;
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
}
复制代码

```

保存之后，样式就自动修改完成了

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f95106fac594e21a566ae298d0504cd~tplv-k3u1fbpfcp-watermark.awebp)

style-loader 核心逻辑相当于：

```
const content = `${样式内容}`
const style = document.createElement('style');
style.innerHTML = content;
document.head.appendChild(style);
复制代码

```

通过动态添加 style 标签的方式，将样式引入页面

#### 1.10 CSS 兼容性

使用 [postcss-loader](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Floaders%2Fpostcss-loader%2F "https://webpack.docschina.org/loaders/postcss-loader/")，自动添加 CSS3 部分属性的浏览器前缀

上面我们用到的 `transform: translateX(-50%);`，需要加上不同的浏览器前缀，这个我们可以使用 postcss-loader 来帮助我们完成

```
npm install postcss-loader postcss -D
复制代码

```

```
const config = {
  // ...
  module: { 
    rules: [
      {
        test: /.css$/, //匹配所有的 css 文件
        use: ['style-loader','css-loader', 'postcss-loader']
      }
    ]
  },
  // ...
}
复制代码

```

> ⚠️ 这里有个很大的坑点：**参考文档配置好后，运行的时候会报错**

```
Error: Loading PostCSS "postcss-import" plugin failed: 
Cannot find module 'postcss-import'
复制代码

```

后面尝试安装插件的集合 `postcss-preset-env` ，然后修改配置为

```
// webpack.config.js
// 失败配置
{
    loader: 'postcss-loader',
    options: {
      postcssOptions: {
        plugins: [
          [
            'postcss-preset-env', 
            {
              // 其他选项
            },
          ],
        ],
      },
    },
},
复制代码

```

运行之后依然会报错，在查阅资料后，终于找到了正确的打开方式，我们重新来一遍 😁

```
npm install postcss postcss-loader postcss-preset-env -D
复制代码

```

添加 postcss-loader 加载器

```
const config = {
  // ...
  module: { 
    rules: [
      {
        test: /.css$/, //匹配所有的 css 文件
        use: [
          'style-loader',
          'css-loader', 
          'postcss-loader'
        ]
      }
    ]
  }, 
  // ...
}
复制代码

```

创建 postcss 配置文件 `postcss.config.js`

```
// postcss.config.js
module.exports = {
  plugins: [require('postcss-preset-env')]
}
复制代码

```

创建 postcss-preset-env 配置文件 `.browserslistrc`

```
# 换行相当于 and
last 2 versions # 回退两个浏览器版本
> 0.5% # 全球超过0.5%人使用的浏览器，可以通过 caniuse.com 查看不同浏览器不同版本占有率
IE 10 # 兼容IE 10
复制代码

```

再尝试运行一下

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b86c4d718c2f4601b02e02c8d49bc7df~tplv-k3u1fbpfcp-watermark.awebp)

前缀自动加上了 👏

如果你对 `.browserslistrc` 不同配置产生的效果感兴趣，可以使用 [autoprefixer](https://link.juejin.cn?target=http%3A%2F%2Fautoprefixer.github.io%2F "http://autoprefixer.github.io/") 进行在线转化查看效果

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e664743f10b5470e9d7152458ba765aa~tplv-k3u1fbpfcp-watermark.awebp?)

#### 1.11 引入 Less 或者 Sass

less 和 sass 同样是 Webpack 无法识别的，需要使用对应的 Loader 来处理一下

<table><thead><tr><th>文件类型</th><th>loader</th></tr></thead><tbody><tr><td>Less</td><td>less-loader</td></tr><tr><td>Sass</td><td>sass-loader node-sass 或 dart-sass</td></tr></tbody></table>

Less 处理相对比较简单，直接添加对应的 Loader 就好了

Sass 不光需要安装 `sass-loader` 还得搭配一个 `node-sass`，这里 `node-sass` 建议用淘宝镜像来安装，npm 安装成功的概率太小了 🤣

这里我们就使用 Sass 来做案例

1.  安装

```
$ npm install sass-loader -D
# 淘宝镜像
$ npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
复制代码

```

2.  新建 `./src/sass.scss`

Sass 文件的后缀可以是 `.scss(常用)` 或者 `.sass`

```
$color: rgb(190, 23, 168);

body {
  p {
    background-color: $color;
    width: 300px;
    height: 300px;
    display: block;
    text-align: center;
    line-height: 300px;
  }
}
复制代码

```

3.  引入 Sass 文件

```
import './main.css';
import './sass.scss' // 引入 Sass 文件


const a = 'Hello ITEM'
console.log(a)
module.exports = a;
复制代码

```

4.  修改配置

```
const config = {
   // ...
   rules: [
      {
        test: /\.(s[ac]|c)ss$/i, //匹配所有的 sass/scss/css 文件
        use: [
          'style-loader',
          'css-loader',
          'postcss-loader',
          'sass-loader', 
        ]
      },
    ]
  },
  // ...
}
复制代码

```

来看一下执行结果

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4a6e15eec0674eb6be0eb8b18b5f8e3f~tplv-k3u1fbpfcp-watermark.awebp)

成功 👏

#### 1.12 分离样式文件

前面，我们都是依赖 `style-loader` 将样式通过 style 标签的形式添加到页面上

但是，更多时候，我们都希望可以通过 CSS 文件的形式引入到页面上

1.  安装 [`mini-css-extract-plugin`](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fplugins%2Fmini-css-extract-plugin%2F "https://webpack.docschina.org/plugins/mini-css-extract-plugin/")

```
$ npm install mini-css-extract-plugin -D
复制代码

```

2.  修改 `webpack.config.js` 配置

```
// ...
// 引入插件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')


const config = {
  // ...
  module: { 
    rules: [
      // ...
      {
        test: /\.(s[ac]|c)ss$/i, //匹配所有的 sass/scss/css 文件
        use: [
          // 'style-loader',
          MiniCssExtractPlugin.loader, // 添加 loader
          'css-loader',
          'postcss-loader',
          'sass-loader', 
        ] 
      },
    ]
  },
  // ...
  plugins:[ // 配置插件
    // ...
    new MiniCssExtractPlugin({ // 添加插件
      filename: '[name].[hash:8].css'
    }),
    // ...
  ]
}

// ...
复制代码

```

3.  查看打包结果

```
dist                    
├─ avatar.d4d42d52.png  
├─ bundle.js            
├─ index.html           
├─ logo.56482c77.png    
└─ main.3bcbae64.css # 生成的样式文件  
复制代码

```

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eeb21ada61c14841bb340f9a8cb36aca~tplv-k3u1fbpfcp-watermark.awebp)

#### 1.13 图片和字体文件

虽然上面在配置开发环境的时候，我们可以通过设置 `contentBase` 去直接读取图片类的静态文件，看一下下面这两种图片使用情况

1.  页面直接引入

```
<!-- 本地可以访问，生产环境会找不到图片 -->
<img src="/logo.png" alt="">
复制代码

```

2.  背景图引入

```
<div id="imgBox"></div>
复制代码

```

```
/* ./src/main.css */
...
#imgBox {
  height: 400px;
  width: 400px;
  background: url('../public/logo.png');
  background-size: contain;
}
复制代码

```

直接会报错

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ed6515db830d41549c457c5e2aa4dd88~tplv-k3u1fbpfcp-watermark.awebp)

所以实际上，Webpack 无法识别图片文件，需要在打包的时候处理一下

常用的处理图片文件的 Loader 包含：

<table><thead><tr><th>Loader</th><th>说明</th></tr></thead><tbody><tr><td>file-loader</td><td>解决图片引入问题，并将图片 copy 到指定目录，默认为 dist</td></tr><tr><td>url-loader</td><td>解依赖 file-loader，当图片小于 limit 值的时候，会将图片转为 base64 编码，大于 limit 值的时候依然是使用 file-loader 进行拷贝</td></tr><tr><td>img-loader</td><td>压缩图片</td></tr></tbody></table>

1.  安装 `file-loader`

```
npm install file-loader -D
复制代码

```

2.  修改配置

```
const config = {
  //...
  module: { 
    rules: [
      {
         // ...
      }, 
      {
        test: /\.(jpe?g|png|gif)$/i, // 匹配图片文件
        use:[
          'file-loader' // 使用 file-loader
        ]
      }
    ]
  },
  // ...
}
复制代码

```

3. 引入图片

```
<!-- ./src/index.html -->
<!DOCTYPE html>
<html lang="en">
...
<body>
  <p></p>
  <div id="imgBox"></div>
</body>
</html>
复制代码

```

样式文件中引入

```
/* ./src/sass.scss */

$color: rgb(190, 23, 168);

body {
  p {
    width: 300px;
    height: 300px;
    display: block;
    text-align: center;
    line-height: 300px;
    background: url('../public/logo.png');
    background-size: contain;
  }
}

复制代码

```

js 文件中引入

```
import './main.css';
import './sass.scss'
import logo from '../public/avatar.png'

const a = 'Hello ITEM'
console.log(a)

const img = new Image()
img.src = logo

document.getElementById('imgBox').appendChild(img)
复制代码

```

启动服务，我们看一下效果

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d1a5bc99ab384641912b82ce921b62c5~tplv-k3u1fbpfcp-watermark.awebp)

显示正常 ✌️

我们可以看到图片文件的名字都已经变了，并且带上了 hash 值，然后我看一下打包目录

```
dist                                     
├─ 56482c77280b3c4ad2f083b727dfcbf9.png  
├─ bundle.js                             
├─ d4d42d529da4b5120ac85878f6f69694.png  
└─ index.html                            
复制代码

```

dist 目录下面多了两个文件，这正是 file-loader 拷贝过来的

如果想要修改一下名称，可以加个配置

```
const config = {
  //...
  module: { 
    rules: [
      {
         // ...
      }, 
      {
        test: /\.(jpe?g|png|gif)$/i,
        use:[
          {
            loader: 'file-loader',
            options: {
              name: '[name][hash:8].[ext]'
            }
          }
        ]
      },
      {
        loader: 'file-loader',
        options: {
          name: '[name][hash:8].[ext]'
        }
      }
    ]
  },
  // ...
}
复制代码

```

再次打包看一下

```
dist                   
├─ avatard4d42d52.png  
├─ bundle.js           
├─ index.html          
└─ logo56482c77.png    
复制代码

```

再看一下 url-loader

4.  安装 `url-loader`

```
$ npm install url-loader -D
复制代码

```

5.  配置 `url-loader`

配置和 file-loader 类似，多了一个 limit 的配置

```
const config = {
  //...
  module: { 
    rules: [
      {
         // ...
      }, 
      {
        test: /\.(jpe?g|png|gif)$/i,
        use:[
          {
            loader: 'url-loader',
            options: {
              name: '[name][hash:8].[ext]',
              // 文件小于 50k 会转换为 base64，大于则拷贝文件
              limit: 50 * 1024
            }
          }
        ]
      },
    ]
  },
  // ...
}
复制代码

```

看一下，我们两个图片文件的体积

```
public         
├─ avatar.png # 167kb
└─ logo.png   # 43kb 
复制代码

```

我们打包看一下效果

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/87741d3bd67a4b7fbe3a8ce626690579~tplv-k3u1fbpfcp-watermark.awebp)

很明显可以看到 logo.png 文件已经转为 base64 了👌

再看字体文件的处理

6.  配置字体文件

首先，从 [iconfont.cn](https://link.juejin.cn?target=https%3A%2F%2Fwww.iconfont.cn%2Fhome%2Findex%3Fspm%3Da313x.7781069.1998910419.2 "https://www.iconfont.cn/home/index?spm=a313x.7781069.1998910419.2") 下载字体文件到本地

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3fca370ecf0c4dfba36feb7385377b77~tplv-k3u1fbpfcp-watermark.awebp)

在项目中，新建 `./src/fonts` 文件夹来存放字体文件

然后，引入到入口文件

```
// ./src/index.js

import './main.css';
import './sass.scss'
import logo from '../public/avatar.png'

// 引入字体图标文件
import './fonts/iconfont.css'

const a = 'Hello ITEM'
console.log(a)

const img = new Image()
img.src = logo

document.getElementById('imgBox').appendChild(img)
复制代码

```

接着，在 `./src/index.html` 中使用

```
<!DOCTYPE html>
<html lang="en">
...
<body>
  <p></p>
  <!-- 使用字体图标文件 -->
  <!-- 1）iconfont 对应 font-family 设置的值-->
  <!-- 2）icon-member 图标 class 名称可以在 iconfont.cn 中查找-->
  <i class="iconfont icon-member"></i>
  <div id="imgBox"></div>
</body>
</html>
复制代码

```

最后，增加字体文件的配置

```
const config = {
  // ...
  {
    test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,  // 匹配字体文件
    use: [
      {
        loader: 'url-loader',
        options: {
          name: 'fonts/[name][hash:8].[ext]', // 体积大于 10KB 打包到 fonts 目录下 
          limit: 10 * 1024,
        } 
      }
    ]
  },
  // ...
}
复制代码

```

打包一下，看看效果

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a0646f552f1458fb290efb2be864360~tplv-k3u1fbpfcp-watermark.awebp)

但是在 webpack5，内置了资源处理模块，`file-loader` 和 `url-loader` 都可以不用安装

#### 1.14 资源模块的使用

> webpack5 新增资源模块 (asset module)，允许使用资源文件（字体，图标等）而无需配置额外的 loader。

资源模块支持以下四个配置：

> 1.  `asset/resource` 将资源分割为单独的文件，并导出 url，类似之前的 file-loader 的功能.
> 2.  `asset/inline` 将资源导出为 dataUrl 的形式，类似之前的 url-loader 的小于 limit 参数时功能.
> 3.  `asset/source` 将资源导出为源码（source code）. 类似的 raw-loader 功能.
> 4.  `asset` 会根据文件大小来选择使用哪种类型，当文件小于 8 KB（默认） 的时候会使用 asset/inline，否则会使用 asset/resource

贴一下修改后的完整代码

```
// ./src/index.js

const config = {
  // ...
  module: { 
    rules: [
      // ... 
      {
        test: /\.(jpe?g|png|gif)$/i,
        type: 'asset',
        generator: {
          // 输出文件位置以及文件名
          // [ext] 自带 "." 这个与 url-loader 配置不同
          filename: "[name][hash:8][ext]"
        },
        parser: {
          dataUrlCondition: {
            maxSize: 50 * 1024 //超过50kb不转 base64
          }
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
        type: 'asset',
        generator: {
          // 输出文件位置以及文件名
          filename: "[name][hash:8][ext]"
        },
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024 // 超过100kb不转 base64
          }
        }
      },
    ]
  },
  // ...
}

module.exports = (env, argv) => {
  console.log('argv.mode=',argv.mode) // 打印 mode(模式) 值
  // 这里可以通过不同的模式修改 config 配置
  return config;
}
复制代码

```

执行打包，结果和之前一样

#### 1.15 JS 兼容性（Babel）

在开发中我们想使用最新的 Js 特性，但是有些新特性的浏览器支持并不是很好，所以 Js 也需要做兼容处理，常见的就是将 ES6 语法转化为 ES5。

这里将登场的 “全场最靓的仔” -- Babel

1.  未配置 Babel

我们写点 ES6 的东西

```
// ./src/index.js

import './main.css';
import './sass.scss'
import logo from '../public/avatar.png'

import './fonts/iconfont.css'

// ...

class Author {
  name = 'ITEM'
  age = 18
  email = 'lxp_work@163.com'

  info =  () => {
    return {
      name: this.name,
      age: this.age,
      email: this.email
    }
  }
}


module.exports = Author

复制代码

```

为了方便看源码，我们把 mode 换成 `development`

接着执行打包命令

打包完成之后，打开 `bundle.js` 查看打包后的结果

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7599da93b146442b89380f1a0b6b60d9~tplv-k3u1fbpfcp-watermark.awebp)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/72c389e772c84716bc2d39d9d3460f63~tplv-k3u1fbpfcp-watermark.awebp)

虽然我们还是可以找打我们的代码，但是阅读起来比较不直观，我们先设置 mode 为 `none`，以最原始的形式打包，再看一下打包结果

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a2d7d5965779444d9a0e547f0643de1f~tplv-k3u1fbpfcp-watermark.awebp)

打包后的代码变化不大，只是对图片地址做了替换，接下来看看配置 babel 后的打包结果会有什么变化

2.  安装依赖

```
$ npm install babel-loader @babel/core @babel/preset-env -D
复制代码

```

*   `babel-loader` 使用 Babel 加载 ES2015+ 代码并将其转换为 ES5
*   `@babel/core` Babel 编译的核心包
*   `@babel/preset-env` Babel 编译的预设，可以理解为 Babel 插件的超集

3.  配置 Babel 预设

```
// webpack.config.js
// ...
const config = {
  entry: './src/index.js', // 打包入口地址
  output: {
    filename: 'bundle.js', // 输出文件名
    path: path.join(__dirname, 'dist'), // 输出文件目录
  },
  module: { 
    rules: [
      {
        test: /\.js$/i,
        use: [
          {
            loader: 'babel-loader',
            options: {
              presets: [
                '@babel/preset-env'
              ],
            }
          }
        ]
      },
    // ...
    ]
  },
  //...
}
// ...
复制代码

```

配置完成之后执行一下打包

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/150bfa173e2640969731294876d75a22~tplv-k3u1fbpfcp-watermark.awebp)

刚才写的 ES6 class 写法 已经转换为了 ES5 的构造函数形式

尽然是做兼容处理，我们自然也可以指定到底要兼容哪些浏览器

为了避免 `webpack.config.js` 太臃肿，建议将 Babel 配置文件提取出来

根目录下新增 `.babelrc.js`

```
// ./babelrc.js

module.exports = {
  presets: [
    [
      "@babel/preset-env",
      {
        // useBuiltIns: false 默认值，无视浏览器兼容配置，引入所有 polyfill
        // useBuiltIns: entry 根据配置的浏览器兼容，引入浏览器不兼容的 polyfill
        // useBuiltIns: usage 会根据配置的浏览器兼容，以及你代码中用到的 API 来进行 polyfill，实现了按需添加
        useBuiltIns: "entry",
        corejs: "3.9.1", // 是 core-js 版本号
        targets: {
          chrome: "58",
          ie: "11",
        },
      },
    ],
  ],
};

复制代码

```

好了，这里一个简单的 Babel 预设就配置完了

常见 Babel 预设还有：

*   `@babel/preset-flow`
*   `@babel/preset-react`
*   `@babel/preset-typescript`

感兴趣的可以自己去了解一下，这里不做扩展了，下面再说说插件的使用

4.  配置 Babel 插件

对于正在提案中，还未进入 ECMA 规范中的新特性，Babel 是无法进行处理的，必须要安装对应的插件，例如：

```
// ./ index.js

import './main.css';
import './sass.scss'
import logo from '../public/avatar.png'

import './fonts/iconfont.css'

const a = 'Hello ITEM'
console.log(a)

const img = new Image()
img.src = logo

document.getElementById('imgBox').appendChild(img)

// 新增装饰器的使用
@log('hi')
class MyClass { }

function log(text) {
  return function(target) {
    target.prototype.logger = () => `${text}，${target.name}`
  }
}

const test = new MyClass()
test.logger()
复制代码

```

执行一下打包

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c4b0bc28b22d4ce0a1ead379099dc888~tplv-k3u1fbpfcp-watermark.awebp)

不出所料，识别不了 🙅🏻‍♀️

怎么才能使用呢？Babel 其实提供了对应的插件：

*   `@babel/plugin-proposal-decorators`
*   `@babel/plugin-proposal-class-properties`

安装一下：

```
$ npm install babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties -D
复制代码

```

打开 `.babelrc.js` 加上插件的配置

```
module.exports = {
  presets: [
    [
      "@babel/preset-env",
      {
        useBuiltIns: "entry",
        corejs: "3.9.1",
        targets: {
          chrome: "58",
          ie: "11",
        },
      },
    ],
  ],
  plugins: [    
    ["@babel/plugin-proposal-decorators", { legacy: true }],
    ["@babel/plugin-proposal-class-properties", { loose: true }],
  ]
};
复制代码

```

这样就可以打包了，在 `bundle.js` 中已经转化为浏览器支持的 Js 代码

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/98984317dfe044e3beb9bcb865115720~tplv-k3u1fbpfcp-watermark.awebp)

同理，我们可以根据自己的实际需求，搭配不同的插件进行使用

### 2 SourceMap 配置选择

SourceMap 是一种映射关系，当项目运行后，如果出现错误，我们可以利用 SourceMap 反向定位到源码位置

#### 2.1 devtool 配置

```
const config = {
  entry: './src/index.js', // 打包入口地址
  output: {
    filename: 'bundle.js', // 输出文件名
    path: path.join(__dirname, 'dist'), // 输出文件目录
  },
  devtool: 'source-map',
  module: { 
     // ...
  }
  // ...
复制代码

```

执行打包后，dist 目录下会生成以 `.map` 结尾的 SourceMap 文件

```
dist                   
├─ avatard4d42d52.png  
├─ bundle.js           
├─ bundle.js.map     
└─ index.html          
复制代码

```

除了 `source-map` 这种类型之外，还有很多种类型可以用，例如：

*   `eval`
*   `eval-source-map`
*   `cheap-source-map`
*   `inline-source-map`
*   `cheap-module-source-map`
*   `inline-cheap-source-map`
*   `cheap-module-eval-source-map`
*   `inline-cheap-module-source-map`
*   `hidden-source-map`
*   `nosources-source-map`

这么多种，到底都有什么区别？如何选择呢？

#### 2.2 配置项差异

1.  为了方便比较它们的不同，我们**新建一个项目**

```
webpack_source_map                                             
├─ src                                      
│  ├─ Author.js                            
│  └─ index.js                               
├─ package.json                             
└─ webpack.config.js                        
复制代码

```

2.  打开 `./src/Author.js`

```
class Author {
  name = 'ITEM'
  age = 18
  email = 'lxp_work@163.com'

  info =  () => {
    return {
      name: this.name,
      age: this.age,
      email: this.email
    }
  }
}

module.exports = Author
复制代码

```

3.  打开 `./src/index.js`

```
import Author from './Author'

const a = 'Hello ITEM'
console.log(a)

const img = new Image()
img.src = logo

document.getElementById('imgBox').appendChild(img)

const author = new Author();

console.log(author.info)
复制代码

```

4.  打开 `package.json`

```
{
  "name": "webpack-source-map",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "build": "webpack"
  },
  "devDependencies": {
    "@babel/core": "^7.6.4",
    "@babel/preset-env": "^7.6.3",
    "babel-loader": "^8.0.6",
    "html-webpack-plugin": "^3.2.0",
    "webpack": "^5.44.0",
    "webpack-cli": "^4.7.2"
  }
}

复制代码

```

5.  打开 `webpack.config.js`

```
// 多入口打包
module.exports = [
  {
    entry: './src/index.js',
    output: {
      filename: 'a.js'
    }
  },
  {
    entry: './src/index.js',
    output: {
      filename: 'b.js'
    }
  }
]
复制代码

```

执行打包命令 `npm run build`，看一下结果

```
dist     
├─ a.js  
└─ b.js  
复制代码

```

不用关心打包结果 `a.js b.js` 里面是什么，到这步的目的是测试多入口打包

改造成多入口的目的是方便我们后面进行比较

6.  不同配置项使用单独的打包入口，打开 `webpack.config.js` 修改

```
const HtmlWebpackPlugin = require('html-webpack-plugin')

// 1）定义不同的打包类型
const allModes = [
  'eval',
  'source-map',
  'eval-source-map',
  'cheap-source-map',
  'inline-source-map',
  'cheap-eval-source-map',
  'cheap-module-source-map',
  'inline-cheap-source-map',
  'cheap-module-eval-source-map',
  'inline-cheap-module-source-map',
  'hidden-source-map',
  'nosources-source-map'
]

// 2）循环不同 SourceMap 模式，生成多个打包入口
module.exports = allModes.map(item => {
  return {
    devtool: item,
    mode: 'none',
    entry: './src/main.js',
    output: {
      filename: `js/${item}.js`
    },
    module: {
      rules: [
        {
          test: /.js$/,
          use: {
            loader: 'babel-loader',
            options: {
              presets: ['@babel/preset-env']
            }
          }
        }
      ]
    },
    plugins: [
      3）输出到不同的页面
      new HtmlWebpackPlugin({
        filename: `${item}.html`
      })
    ]
  }
}
复制代码

```

7.  模拟代码错误

```
// ./src/index.js

import Author from './Author'

const a = 'Hello ITEM'
// 故意使用了错误的 console.log 
console.log11(a)
 
const img = new Image()
img.src = logo

document.getElementById('imgBox').appendChild(img)

const author = new Author();

console.log(author.info)
复制代码

```

8.  尝试打包

报错了!!

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a867a6e05a8c4f199ca9958825b21b4f~tplv-k3u1fbpfcp-watermark.awebp)

提示有 SourceMap 模式的名称不对，原来它们的拼接是有规律和意义的

我们按照校验规则 `^(inline-|hidden-|eval-)?(nosources-)?(cheap-(module-)?)?source-map$` 检查一下

`cheap-eval-source-map` 和 `cheap-module-eval-source-map` 好像有问题，`eval` 跑后面去了，改一下

```
// 修改之后
const allModes = [
  'eval',
  'source-map',
  'eval-source-map',
  'cheap-source-map',
  'inline-source-map',
  'eval-cheap-source-map',
  'cheap-module-source-map',
  'inline-cheap-source-map',
  'eval-cheap-module-source-map',
  'inline-cheap-module-source-map',
  'hidden-source-map',
  'nosources-source-map'
]
复制代码

```

再执行一下打包

还是有报错！！接着改

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/af32b5fa675e45c8880d637ccbba1681~tplv-k3u1fbpfcp-watermark.awebp)

错误信息我查了一下，大概率是 `html-webpack-pugin` 的版本太老，不兼容 webpack5，我们升级一下版本至 `"html-webpack-plugin": "^5.3.2"` 再尝试一下，OK 了

```
dist                                     
├─ js                                    
│  ├─ cheap-module-source-map.js #............ 有对应的 .map 文件        
│  ├─ cheap-module-source-map.js.map     
│  ├─ cheap-source-map.js #................... 有              
│  ├─ cheap-source-map.js.map            
│  ├─ eval-cheap-module-source-map.js #....... 无  
│  ├─ eval-cheap-source-map.js #.............. 无          
│  ├─ eval-source-map.js #.................... 无               
│  ├─ eval.js #............................... 无                           
│  ├─ hidden-source-map.js #.................. 有              
│  ├─ hidden-source-map.js.map           
│  ├─ inline-cheap-module-source-map.js #..... 无  
│  ├─ inline-cheap-source-map.js #............ 无         
│  ├─ inline-source-map.js #.................. 无               
│  ├─ nosources-source-map.js #............... 有           
│  ├─ nosources-source-map.js.map        
│  ├─ source-map.js #......................... 有                     
│  └─ source-map.js.map                  
├─ cheap-module-source-map.html          
├─ cheap-source-map.html                 
├─ eval-cheap-module-source-map.html     
├─ eval-cheap-source-map.html            
├─ eval-source-map.html                  
├─ eval.html                             
├─ hidden-source-map.html                
├─ inline-cheap-module-source-map.html   
├─ inline-cheap-source-map.html          
├─ inline-source-map.html                
├─ nosources-source-map.html             
└─ source-map.html                       
复制代码

```

从目录结构我们可以很容易看出来，含 `eval` 和 `inline` 模式的都没有对应的`.map` 文件，具体为什么，下面接着分析

接着，我们在 dist 目录起一个服务，在浏览器打开

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4201c3390f4242adbb92be333f6159cb~tplv-k3u1fbpfcp-watermark.awebp)

然后，我们一个个来分析

**`eval` 模式：**

1.  生成代码通过 **`eval`** 执行 👇🏻

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/14663eba6d0d4f66b12586ecda2b2616~tplv-k3u1fbpfcp-watermark.awebp)

2.  源代码位置通过 **`@sourceURL`** 注明 👇🏻

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23343307c41c4e9cba4e7912e4117af4~tplv-k3u1fbpfcp-watermark.awebp)

3.  无法定位到错误位置，只能定位到**某个文件**
    
4.  不用生成 SourceMap 文件，**打包速度快**
    

**`source-map` 模式：**

1.  生成了对应的 SourceMap 文件，**打包速度慢**
2.  在**源代码**中定位到错误所在**行列**信息 👇🏻

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d0ac476816cc455eb29264c51cbcd434~tplv-k3u1fbpfcp-watermark.awebp)

**`eval-source-map` 模式：**

1.  生成代码通过 **`eval` 执行** 👇🏻

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c61268e879747b78db39d6f4a58adac~tplv-k3u1fbpfcp-watermark.awebp) 2. 包含 **dataUrl** 形式的 SourceMap 文件

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b23cb999e644e3aba37182e2da1ebd4~tplv-k3u1fbpfcp-watermark.awebp)

3.  可以在**编译后**的代码中定位到错误所在**行列**信息

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d1c061a71274b3182b1ad9fa2cb55d3~tplv-k3u1fbpfcp-watermark.awebp)

4.  生成 dataUrl 形式的 SourceMap，打包速度慢

**`eval-cheap-source-map` 模式：**

1.  生成代码通过 **`eval` 执行**
2.  包含 **dataUrl** 形式的 SourceMap 文件
3.  可以在**编译后**的代码中定位到错误所在**行**信息
4.  不需要定位列信息，打包**速度较快**

**`eval-cheap-module-source-map` 模式：**

1.  生成代码通过 **`eval`** 执行
2.  包含 **dataUrl** 形式的 SourceMap 文件
3.  可以在**编译后**的代码中定位到错误所在**行**信息
4.  不需要定位列信息，打包**速度较快**
5.  在**源代码**中定位到错误所在**行**信息 👇🏻

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1b4a0ab355de438098f544fd339bd5c6~tplv-k3u1fbpfcp-watermark.awebp)

**`inline-source-map` 模式：**

1.  通过 **dataUrl** 的形式引入 SourceMap 文件 👇🏻

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/127071e076ce40d0a9567d8a9ff2128d~tplv-k3u1fbpfcp-watermark.awebp)

... 余下和 `source-map` 模式一样

**`hidden-source-map` 模式：**

1.  看不到 SourceMap 效果，但是生成了 SourceMap 文件

**`nosources-source-map` 模式：**

1.  能看到错误出现的位置 👇🏻

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1935a054b1dc47f7b2a0156a635a8ce7~tplv-k3u1fbpfcp-watermark.awebp)

2.  但是没有办法现实对应的源码

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a64e5446bd0c454091db7b72864a7504~tplv-k3u1fbpfcp-watermark.awebp)

接下来，我们稍微总结一下：

<table><thead><tr><th>devtool</th><th>build</th><th>rebuild</th><th>显示代码</th><th>SourceMap 文件</th><th>描述</th></tr></thead><tbody><tr><td>(none)</td><td>很快</td><td>很快</td><td>无</td><td>无</td><td>无法定位错误</td></tr><tr><td>eval</td><td>快</td><td>很快（cache）</td><td>编译后</td><td>无</td><td>定位到文件</td></tr><tr><td>source-map</td><td>很慢</td><td>很慢</td><td>源代码</td><td>有</td><td>定位到行列</td></tr><tr><td>eval-source-map</td><td>很慢</td><td>一般（cache）</td><td>编译后</td><td>有（dataUrl）</td><td>定位到行列</td></tr><tr><td>eval-cheap-source-map</td><td>一般</td><td>快（cache）</td><td>编译后</td><td>有（dataUrl）</td><td>定位到行</td></tr><tr><td>eval-cheap-module-source-map</td><td>慢</td><td>快（cache）</td><td>源代码</td><td>有（dataUrl）</td><td>定位到行</td></tr><tr><td>inline-source-map</td><td>很慢</td><td>很慢</td><td>源代码</td><td>有（dataUrl）</td><td>定位到行列</td></tr><tr><td>hidden-source-map</td><td>很慢</td><td>很慢</td><td>源代码</td><td>有</td><td>无法定位错误</td></tr><tr><td>nosource-source-map</td><td>很慢</td><td>很慢</td><td>源代码</td><td>无</td><td>定位到文件</td></tr></tbody></table>

对照一下校验规则 `^(inline-|hidden-|eval-)?(nosources-)?(cheap-(module-)?)?source-map$` 分析一下关键字

<table><thead><tr><th>关键字</th><th>描述</th></tr></thead><tbody><tr><td>inline</td><td>代码内通过 dataUrl 形式引入 SourceMap</td></tr><tr><td>hidden</td><td>生成 SourceMap 文件，但不使用</td></tr><tr><td>eval</td><td><code>eval(...)</code> 形式执行代码，通过 dataUrl 形式引入 SourceMap</td></tr><tr><td>nosources</td><td>不生成 SourceMap</td></tr><tr><td>cheap</td><td>只需要定位到行信息，不需要列信息</td></tr><tr><td>module</td><td>展示源代码中的错误位置</td></tr></tbody></table>

好了，到这里 SourceMap 就分析完了

#### 2.3 推荐配置

1.  本地开发：

推荐：`eval-cheap-module-source-map`

理由：

*   本地开发首次打包慢点没关系，因为 `eval` 缓存的原因，rebuild 会很快
*   开发中，我们每行代码不会写的太长，只需要定位到行就行，所以加上 `cheap`
*   我们希望能够找到源代码的错误，而不是打包后的，所以需要加上 `modele`

2.  生产环境：

推荐：`(none)`

理由：

*   就是不想别人看到我的源代码

### 3. 三种 hash 值

Webpack 文件指纹策略是将文件名后面加上 hash 值。特别在使用 CDN 的时候，缓存是它的特点与优势，但如果打包的文件名，没有 hash 后缀的话，你肯定会被缓存折磨的够呛 😂

例如我们在基础配置中用到的：`filename: "[name][hash:8][ext]"`

这里里面 `[]` 包起来的，就叫占位符，它们都是什么意思呢？请看下面这个表 👇🏻

<table><thead><tr><th>占位符</th><th>解释</th></tr></thead><tbody><tr><td>ext</td><td>文件后缀名</td></tr><tr><td>name</td><td>文件名</td></tr><tr><td>path</td><td>文件相对路径</td></tr><tr><td>folder</td><td>文件所在文件夹</td></tr><tr><td>hash</td><td>每次构建生成的唯一 hash 值</td></tr><tr><td>chunkhash</td><td>根据 chunk 生成 hash 值</td></tr><tr><td>contenthash</td><td>根据文件内容生成 hash 值</td></tr></tbody></table>

表格里面的 `hash`、`chunkhash`、`contenthash` 你可能还是不清楚差别在哪

*   **hash** ：任何一个文件改动，整个项目的构建 hash 值都会改变；
*   **chunkhash**：文件的改动只会影响其所在 chunk 的 hash 值；
*   **contenthash**：每个文件都有单独的 hash 值，文件的改动只会影响自身的 hash 值；

二、Webpack 进阶
------------

第二部分，我们将向 “能优化” 的方向前进 🏃

除了配置上的优化外，我们也要学习如何自己开发 Loader 和 Plugin

### 1. 优化构建速度

#### 1.1 构建费时分析

这里我们需要使用插件 [speed-measure-webpack-plugin](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fspeed-measure-webpack-plugin "https://www.npmjs.com/package/speed-measure-webpack-plugin")，我们参考文档配置一下

1.  首先安装一下

```
$ npm i -D speed-measure-webpack-plugin
复制代码

```

2.  修改我们的配置文件 webpack.config.js

```
...
// 费时分析
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();
...

const config = {...}

module.exports = (env, argv) => {
  // 这里可以通过不同的模式修改 config 配置


  return smp.wrap(config);
}
复制代码

```

3.  执行打包

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ae510d710d494d22a00378d8d52753d2~tplv-k3u1fbpfcp-watermark.awebp?)

报错了🤦🏻‍♂️

这里就暴露了使用这个插件的一个弊端，就是：

*   **有些 Loader 或者 Plugin 新版本会不兼容，需要进行降级处理**

这里我们对 `mini-css-extract-plugin` 进行一下降级处理: `^2.1.0` -> `^1.3.6`

重新安装一下依赖，再次执行打包

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e369fd6b2994270971fd87291e694f6~tplv-k3u1fbpfcp-watermark.awebp?)

降了版本之后，还是报错，根据提示信息，我们给配置加上 `publicPath: './'`

```
output: {
    filename: 'bundle.js', // 输出文件名
    path: path.join(__dirname, 'dist'), // 输出文件目录
    publicPath: './'
  },
复制代码

```

在尝试一次

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8abe8e0dfeb94ffe94adce229d04d0e3~tplv-k3u1fbpfcp-watermark.awebp?)

成功了！

> **注意：在 webpack5.x 中为了使用费时分析去对插件进行降级或者修改配置写法是非常不划算的**，这里因为演示需要，我后面会继续使用，但是在平时开发中，建议还是不要使用。

#### 1.2 优化 resolve 配置

##### 1.2.1 alias

alias 用的创建 `import` 或 `require` 的别名，用来简化模块引用，项目中基本都需要进行配置。

```
const path = require('path')
...
// 路径处理方法
function resolve(dir){
  return path.join(__dirname, dir);
}

 const config  = {
  ...
  resolve:{
    // 配置别名
    alias: {
      '~': resolve('src'),
      '@': resolve('src'),
      'components': resolve('src/components'),
    }
  }
};
复制代码

```

配置完成之后，我们在项目中就可以

```
// 使用 src 别名 ~ 
import '~/fonts/iconfont.css'

// 使用 src 别名 @ 
import '@/fonts/iconfont.css'

// 使用 components 别名
import footer from "components/footer";
复制代码

```

##### 1.2.2 extensions

webpack 默认配置

```
const config = {
  //...
  resolve: {
    extensions: ['.js', '.json', '.wasm'],
  },
};
复制代码

```

如果用户引入模块时不带扩展名，例如

```
import file from '../path/to/file';
复制代码

```

那么 webpack 就会按照 extensions 配置的数组从左到右的顺序去尝试解析模块

需要注意的是：

1.  高频文件后缀名放前面；
2.  手动配置后，默认配置会被覆盖

如果想保留默认配置，可以用 `...` 扩展运算符代表默认配置，例如

```
const config = {
  //...
  resolve: {
    extensions: ['.ts', '...'], 
  },
};
复制代码

```

##### 1.2.3 modules

告诉 webpack 解析模块时应该搜索的目录，常见配置如下

```
const path = require('path');

// 路径处理方法
function resolve(dir){
  return path.join(__dirname, dir);
}

const config = {
  //...
  resolve: {
     modules: [resolve('src'), 'node_modules'],
  },
};
复制代码

```

告诉 webpack 优先 src 目录下查找需要解析的文件，会大大节省查找时间

##### 1.2.4 resolveLoader

resolveLoader 与上面的 resolve 对象的属性集合相同， 但仅用于解析 webpack 的 [loader](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fconcepts%2Floaders "https://webpack.docschina.org/concepts/loaders") 包。

**一般情况下保持默认配置就可以了，但如果你有自定义的 Loader 就需要配置一下**，不配可能会因为找不到 loader 报错

*   例如：我们在 loader 文件夹下面，放着我们自己写的 loader

我们就可以怎么配置

```
const path = require('path');

// 路径处理方法
function resolve(dir){
  return path.join(__dirname, dir);
}

const config = {
  //...
  resolveLoader: {
    modules: ['node_modules',resolve('loader')]
  },
};
复制代码

```

#### 1.3 externals

`externals` 配置选项提供了「**从输出的 bundle 中排除依赖**」的方法。此功能通常对 **library 开发人员**来说是最有用的，然而也会有各种各样的应用程序用到它。

例如，从 CDN 引入 jQuery，而不是把它打包：

1.  引入链接

```
<script
  src="https://code.jquery.com/jquery-3.1.0.js"
  integrity="sha256-slogkvB1K3VOkzAI8QITxV3VzpOnkeNVsKvtkYLMjfk="
  crossorigin="anonymous"
></script>
复制代码

```

2.  配置 externals

```
const config = {
  //...
  externals: {
    jquery: 'jQuery',
  },
};
复制代码

```

3.  使用 jQuery

```
import $ from 'jquery';

$('.my-element').animate(/* ... */);
复制代码

```

我们可以用这样的方法来剥离不需要改动的一些依赖，大大节省打包构建的时间。

#### 1.3 缩小范围

在配置 loader 的时候，我们需要更精确的去指定 loader 的作用目录或者需要排除的目录，通过使用 `include` 和 `exclude` 两个配置项，可以实现这个功能，常见的例如：

*   **`include`**：符合条件的模块进行解析
*   **`exclude`**：排除符合条件的模块，不解析
*   **`exclude`** 优先级更高

例如在配置 babel 的时候

```
const path = require('path');

// 路径处理方法
function resolve(dir){
  return path.join(__dirname, dir);
}

const config = {
  //...
  module: { 
    noParse: /jquery|lodash/,
    rules: [
      {
        test: /\.js$/i,
        include: resolve('src'),
        exclude: /node_modules/,
        use: [
          'babel-loader',
        ]
      },
      // ...
    ]
  }
};
复制代码

```

#### 1.3 noParse

*   不需要解析依赖的第三方大型类库等，可以通过这个字段进行配置，以提高构建速度
*   使用 noParse 进行忽略的模块文件中不会解析 `import`、`require` 等语法

```
const config = {
  //...
  module: { 
    noParse: /jquery|lodash/,
    rules:[...]
  }

};
复制代码

```

#### 1.4 IgnorePlugin

防止在 `import` 或 `require` 调用时，生成以下正则表达式匹配的模块：

*   `requestRegExp` 匹配 (test) 资源请求路径的正则表达式。
*   `contextRegExp` 匹配 (test) 资源上下文（目录）的正则表达式。

```
new webpack.IgnorePlugin({ resourceRegExp, contextRegExp });
复制代码

```

以下示例演示了此插件的几种用法。

1.  安装 moment 插件（时间处理库）

```
$ npm i -S moment
复制代码

```

2.  配置 IgnorePlugin

```
// 引入 webpack
const webpack = require('webpack')

const config = {
  ...
  plugins:[ // 配置插件
    ...
    new webpack.IgnorePlugin({
      resourceRegExp: /^\.\/locale$/,
      contextRegExp: /moment$/,
    }),
  ]  
};
复制代码

```

目的是将插件中的非中文语音排除掉，这样就可以大大节省打包的体积了

#### 1.5 多进程配置

> **注意**：实际上在小型项目中，开启多进程打包反而会增加时间成本，因为启动进程和进程间通信都会有一定开销。

##### 1.5.1 thread-loader

配置在 [thread-loader](https://link.juejin.cn/?target=https%3A%2F%2Fwebpack.docschina.org%2Floaders%2Fthread-loader%2F%23root "https://link.juejin.cn/?target=https%3A%2F%2Fwebpack.docschina.org%2Floaders%2Fthread-loader%2F%23root") 之后的 loader 都会在一个单独的 worker 池（worker pool）中运行

1.  安装

```
$ npm i -D  thread-loader
复制代码

```

2.  配置

```
const path = require('path');

// 路径处理方法
function resolve(dir){
  return path.join(__dirname, dir);
}

const config = {
  //...
  module: { 
    noParse: /jquery|lodash/,
    rules: [
      {
        test: /\.js$/i,
        include: resolve('src'),
        exclude: /node_modules/,
        use: [
          {
            loader: 'thread-loader', // 开启多进程打包
            options: {
              worker: 3,
            }
          },
          'babel-loader',
        ]
      },
      // ...
    ]
  }
};
复制代码

```

##### 1.5.2 happypack ❌

同样为开启多进程打包的工具，webpack5 已弃用。

#### 1.6 利用缓存

利用缓存可以大幅提升重复构建的速度

##### 1.6.1 babel-loader 开启缓存

*   babel 在转译 js 过程中时间开销比价大，将 babel-loader 的执行结果缓存起来，重新打包的时候，直接读取缓存
*   缓存位置： `node_modules/.cache/babel-loader`

具体配置如下：

```
const config = {
 module: { 
    noParse: /jquery|lodash/,
    rules: [
      {
        test: /\.js$/i,
        include: resolve('src'),
        exclude: /node_modules/,
        use: [
          // ...
          {
            loader: 'babel-loader',
            options: {
              cacheDirectory: true // 启用缓存
            }
          },
        ]
      },
      // ...
    ]
  }
}
复制代码

```

那其他的 loader 如何将结果缓存呢？

[cache-loader](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fcache-loader "https://www.npmjs.com/package/cache-loader") 就可以帮我们完成这件事情

##### 1.6.2 cache-loader

*   缓存一些性能开销比较大的 loader 的处理结果
*   缓存位置：`node_modules/.cache/cache-loader`

1.  安装

```
$ npm i -D cache-loader
复制代码

```

2.  配置 cache-loader

```
const config = {
 module: { 
    // ...
    rules: [
      {
        test: /\.(s[ac]|c)ss$/i, //匹配所有的 sass/scss/css 文件
        use: [
          // 'style-loader',
          MiniCssExtractPlugin.loader,
          'cache-loader', // 获取前面 loader 转换的结果
          'css-loader',
          'postcss-loader',
          'sass-loader', 
        ]
      }, 
      // ...
    ]
  }
}
复制代码

```

##### 1.6.3 hard-source-webpack-plugin

*   [hard-source-webpack-plugin](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmzgoddard%2Fhard-source-webpack-plugin "https://github.com/mzgoddard/hard-source-webpack-plugin") 为模块提供了中间缓存，重复构建时间大约可以减少 80%，但是在 **webpack5 中已经内置了模块缓存，不需要再使用此插件**

##### 1.6.4 dll ❌

在 webpack5.x 中已经不建议使用这种方式进行模块缓存，因为其已经内置了更好体验的 cache 方法

##### 1.6.5 cache 持久化缓存

通过配置 [cache](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fconfiguration%2Fcache%2F%23root "https://webpack.docschina.org/configuration/cache/#root") 缓存生成的 webpack 模块和 chunk，来改善构建速度。

```
const config = {
  cache: {
    type: 'filesystem',
  },
};
复制代码

```

### 2. 优化构建结果

#### 2.1 构建结果分析

借助插件 [webpack-bundle-analyzer](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fwebpack-bundle-analyzer "https://www.npmjs.com/package/webpack-bundle-analyzer") 我们可以直观的看到打包结果中，文件的体积大小、各模块依赖关系、文件是够重复等问题，极大的方便我们在进行项目优化的时候，进行问题诊断。

1.  安装

```
$ npm i -D webpack-bundle-analyzer
复制代码

```

2.  配置插件

```
// 引入插件
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin


const config = {
  // ...
  plugins:[ 
    // ...
    // 配置插件 
    new BundleAnalyzerPlugin({
      // analyzerMode: 'disabled',  // 不启动展示打包报告的http服务器
      // generateStatsFile: true, // 是否生成stats.json文件
    })
  ],
};
复制代码

```

3.  修改启动命令

```
 "scripts": {
    // ...
    "analyzer": "cross-env NODE_ENV=prod webpack --progress --mode production"
  },
复制代码

```

4.  执行编译命令 `npm run analyzer`

打包结束后，会自行启动地址为 `http://127.0.0.1:8888` 的 web 服务，访问地址就可以看到

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ee392f0838bd43e5aeeb405c76f2fbc7~tplv-k3u1fbpfcp-watermark.awebp?)

如果，我们只想保留数据不想启动 web 服务，这个时候，我们可以加上两个配置

```
new BundleAnalyzerPlugin({
   analyzerMode: 'disabled',  // 不启动展示打包报告的http服务器
   generateStatsFile: true, // 是否生成stats.json文件
})
复制代码

```

这样再次执行打包的时候就只会产生 state.json 的文件了

#### 2.2 压缩 CSS

1.  安装 [`optimize-css-assets-webpack-plugin`](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Foptimize-css-assets-webpack-plugin "https://www.npmjs.com/package/optimize-css-assets-webpack-plugin")

```
$ npm install -D optimize-css-assets-webpack-plugin 
复制代码

```

2.  修改 `webapck.config.js` 配置

```
// ...
// 压缩css
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin')
// ...

const config = {
  // ...
  optimization: {
    minimize: true,
    minimizer: [
      // 添加 css 压缩配置
      new OptimizeCssAssetsPlugin({}),
    ]
  },
 // ...
}

// ...
复制代码

```

3.  查看打包结果

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b3174e204994b2e9c845f5bbc144577~tplv-k3u1fbpfcp-watermark.awebp)

#### 2.3 压缩 JS

> 在生成环境下打包默认会开启 js 压缩，但是当我们手动配置 `optimization` 选项之后，就不再默认对 js 进行压缩，需要我们手动去配置。

因为 webpack5 内置了 [terser-webpack-plugin](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fterser-webpack-plugin "https://www.npmjs.com/package/terser-webpack-plugin") 插件，所以我们不需重复安装，直接引用就可以了，具体配置如下

```
const TerserPlugin = require('terser-webpack-plugin');

const config = {
  // ...
  optimization: {
    minimize: true, // 开启最小化
    minimizer: [
      // ...
      new TerserPlugin({})
    ]
  },
  // ...
}
复制代码

```

#### 2.4 清除无用的 CSS

[purgecss-webpack-plugin](https://link.juejin.cn?target=https%3A%2F%2Fwww.purgecss.cn%2Fplugins%2Fwebpack.html%23%25E7%2594%25A8%25E6%25B3%2595 "https://www.purgecss.cn/plugins/webpack.html#%E7%94%A8%E6%B3%95") 会单独提取 CSS 并清除用不到的 CSS

1.  安装插件

```
$ npm i -D purgecss-webpack-plugin
复制代码

```

2.  添加配置

```
// ...
const PurgecssWebpackPlugin = require('purgecss-webpack-plugin')
const glob = require('glob'); // 文件匹配模式
// ...

function resolve(dir){
  return path.join(__dirname, dir);
}

const PATHS = {
  src: resolve('src')
}

const config = {
  plugins:[ // 配置插件
    // ...
    new PurgecssPlugin({
      paths: glob.sync(`${PATHS.src}/**/*`, {nodir: true})
    }),
  ]
}

复制代码

```

#### 2.5 Tree-shaking

Tree-shaking 作用是剔除没有使用的代码，以降低包的体积

*   webpack 默认支持，需要在 .bablerc 里面设置 `model：false`，即可在生产环境下默认开启

了解更多 Tree-shaking 知识，推荐阅读 👉🏻 [从过去到现在，聊聊 Tree-shaking](https://link.juejin.cn?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FTNXO2ifPymaTxIqzBAmkSQ "https://mp.weixin.qq.com/s/TNXO2ifPymaTxIqzBAmkSQ")

```
module.exports = {
  presets: [
    [
      "@babel/preset-env",
      {
        module: false,
        useBuiltIns: "entry",
        corejs: "3.9.1",
        targets: {
          chrome: "58",
          ie: "11",
        },
      },
    ],
  ],
  plugins: [    
    ["@babel/plugin-proposal-decorators", { legacy: true }],
    ["@babel/plugin-proposal-class-properties", { loose: true }],
  ]
};
复制代码

```

#### 2.6 Scope Hoisting

Scope Hoisting 即作用域提升，原理是将多个模块放在同一个作用域下，并重命名防止命名冲突，**通过这种方式可以减少函数声明和内存开销**。

*   webpack 默认支持，在生产环境下默认开启
*   只支持 es6 代码

### 3. 优化运行时体验

运行时优化的核心就是提升首屏的加载速度，主要的方式就是

*   降低首屏加载文件体积，首屏不需要的文件进行预加载或者按需加载

#### 3.1 入口点分割

配置多个打包入口，多页打包，这里不过多介绍

#### 3.2 splitChunks 分包配置

optimization.splitChunks 是基于 [SplitChunksPlugin](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fplugins%2Fsplit-chunks-plugin%2F "https://webpack.docschina.org/plugins/split-chunks-plugin/") 插件实现的

默认情况下，它只会影响到按需加载的 chunks，因为修改 initial chunks 会影响到项目的 HTML 文件中的脚本标签。

webpack 将根据以下条件自动拆分 chunks：

*   新的 chunk 可以被共享，或者模块来自于 `node_modules` 文件夹
*   新的 chunk 体积大于 20kb（在进行 min+gz 之前的体积）
*   当按需加载 chunks 时，并行请求的最大数量小于或等于 30
*   当加载初始化页面时，并发请求的最大数量小于或等于 30

1.  默认配置介绍

```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async', // 有效值为 `all`，`async` 和 `initial`
      minSize: 20000, // 生成 chunk 的最小体积（≈ 20kb)
      minRemainingSize: 0, // 确保拆分后剩余的最小 chunk 体积超过限制来避免大小为零的模块
      minChunks: 1, // 拆分前必须共享模块的最小 chunks 数。
      maxAsyncRequests: 30, // 最大的按需(异步)加载次数
      maxInitialRequests: 30, // 打包后的入口文件加载时，还能同时加载js文件的数量（包括入口文件）
      enforceSizeThreshold: 50000,
      cacheGroups: { // 配置提取模块的方案
        defaultVendors: {
          test: /[\/]node_modules[\/]/,
          priority: -10,
          reuseExistingChunk: true,
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true,
        },
      },
    },
  },
};
复制代码

```

2.  项目中的使用

```
const config = {
  //...
  optimization: {
    splitChunks: {
      cacheGroups: { // 配置提取模块的方案
        default: false,
        styles: {
            name: 'styles',
            test: /\.(s?css|less|sass)$/,
            chunks: 'all',
            enforce: true,
            priority: 10,
          },
          common: {
            name: 'chunk-common',
            chunks: 'all',
            minChunks: 2,
            maxInitialRequests: 5,
            minSize: 0,
            priority: 1,
            enforce: true,
            reuseExistingChunk: true,
          },
          vendors: {
            name: 'chunk-vendors',
            test: /[\\/]node_modules[\\/]/,
            chunks: 'all',
            priority: 2,
            enforce: true,
            reuseExistingChunk: true,
          },
         // ... 根据不同项目再细化拆分内容
      },
    },
  },
}
复制代码

```

#### 3.3 代码懒加载

针对首屏加载不太需要的一些资源，我们可以通过懒加载的方式去实现，下面看一个小🌰

*   需求：点击图片给图片加一个描述

**1. 新建图片描述信息**

**desc.js**

```
const ele = document.createElement('div')
ele.innerHTML = '我是图片描述'
module.exports = ele
复制代码

```

**2. 点击图片引入描述**

**index.js**

```
import './main.css';
import './sass.scss'
import logo from '../public/avatar.png'

import '@/fonts/iconfont.css'

const a = 'Hello ITEM'
console.log(a)

const img = new Image()
img.src = logo

document.getElementById('imgBox').appendChild(img)

// 按需加载
img.addEventListener('click', () => {
  import('./desc').then(({ default: element }) => {
    console.log(element)
    document.body.appendChild(element)
  })
})
复制代码

```

**3. 查看效果**

*   点击前

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cdeeabe18da4e5f93edc40e429b30c8~tplv-k3u1fbpfcp-watermark.awebp?)

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c9ac178ffa340b8824e0dc18f8a6a1a~tplv-k3u1fbpfcp-watermark.awebp?)

*   点击后

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d1f0d664c3e34deda3e8270ce20f6116~tplv-k3u1fbpfcp-watermark.awebp?)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e2f2be3f57164f06863b0283ea396e12~tplv-k3u1fbpfcp-watermark.awebp?)

#### 3.4 prefetch 与 preload

上面我们使用异步加载的方式引入图片的描述，但是如果需要异步加载的文件比较大时，在点击的时候去加载也会影响到我们的体验，这个时候我们就可以考虑使用 prefetch 来进行预拉取

##### 3.4.1 prefetch

> *   **prefetch** (预获取)：浏览器空闲的时候进行资源的拉取

改造一下上面的代码

```
// 按需加载
img.addEventListener('click', () => {
  import( /* webpackPrefetch: true */ './desc').then(({ default: element }) => {
    console.log(element)
    document.body.appendChild(element)
  })
})
复制代码

```

##### 3.4.2 preload

> *   **preload** (预加载)：提前加载后面会用到的关键资源
> *   ⚠️ 因为会提前拉取资源，如果不是特殊需要，谨慎使用

官网示例：

```
import(/* webpackPreload: true */ 'ChartingLibrary');
复制代码

```

### 4. 手写 Loader

TODO

### 5. 手写 Plugin

TODO

月更博主失踪之谜
--------

其实从 8 月份就打算整理一篇关与 webpack 知识体系的问题，没想到一整理就是好几个月 😂

而且还没有整理完 😅

为了避免文章最后烂尾，还是决定分段的进行更新

下一篇将会补充：**《webpack 深入篇》**

大致内容包括：

*   Webpack 调试
*   Webpack 构建流程
*   热更新（HRM）原理
*   Webpack 核心库 Tapabel 介绍
*   tree-shaking 原理
*   babel & AST 语法树

点赞、关注、评论，支持一波
-------------

您的支持，是我写作的动力，**点赞、关注、评论，支持一波** 👍

关注我的公众号：**前端搬砖工**，一起构建知识体系

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/45ecc68333004a92bd61174cbc9f0b95~tplv-k3u1fbpfcp-watermark.awebp?)

参考资料
----

*   [webpack 官方文档](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2F "https://webpack.docschina.org/")
*   [postcss-loader](https://link.juejin.cn?target=https%3A%2F%2Fbbs.huaweicloud.com%2Fblogs%2F243687 "https://bbs.huaweicloud.com/blogs/243687")
*   [file-loader](https://juejin.cn/post/6971743815434993671 "https://juejin.cn/post/6971743815434993671")
*   [学习 Webpack5 之路（优化篇）](https://juejin.cn/post/6996816316875161637 "https://juejin.cn/post/6996816316875161637")