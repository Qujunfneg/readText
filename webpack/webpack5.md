# webpack5成神之路

[TOC]

#### 安装

```javascript
npm i webpack webpack-cli --save-dev
```

***敲黑板***  `webpack`默认入口为`src`下的`index.js`，出口为`dist`下的`main.js`
***敲黑板***  没有了解过`npx`的可以先去了解`npx`命令



#### 执行

```javascript
npx webapck  //会执行默认操作,将src/index打包到dist/main.js
// 可以将命令放置package.json中,
// sciprits:{
//	 "start":"npx webpack"
//}
```

#### 配置文件

```javascript
// 根目录下增加webpack.config.js文件
// 核心配置
var path = require('path')
module.exports = {
    entry:'./src/index.js',  // 入口
    output:{
        filename:'bundle.js', // 打包后的文件名称
        path:path.resolve(__dirname,'dist') // 打包路径
    }
}
```

#### 管理资源

##### 加载`css`资源

```javascript
// 前提需要安装 css-loader 和 style-loader,并在module配置增加
// npm install css-loader style-loader -D
// 测试都在src/index通过import方式引入
```

```javascript
// webpack.config.js
var path = require('path')
module.exports = {
    entry:'./src/index.js',  // 入口
    output:{
        filename:'bundle.js', // 打包后的文件名称
        path:path.resolve(__dirname,'dist') // 打包路径
    },
    module:{
        rules:[
            {
                test:/\.css$/i,
                use:['style-loader','css-loader']
            }
        ]
    }
}
```

##### 加载image图像

```javascript
// webpack5利用内置的Asset Modules 可以处理，当然也可使用file-loader和url-loader
// file-loader和url-loader区别：
// url-loader封装了file-loader。url-loader不依赖于file-loader，即使用url-loader时，只需要安装url-loader即可，不需要安装file-loader，因为url-loader内置了file-loader。

// url-loader解决的问题:如果图片较多，会发很多http请求，会降低页面性能。url-loader会将引入的图片编码，生成dataURl。相当于把图片数据翻译成一串字符。再把这串字符打包到文件中，最终只需要引入这个文件就能访问图片了。当然，如果图片较大，编码会消耗性能。因此url-loader提供了一个limit参数，小于limit字节的文件会被转为DataURl，大于limit的还会使用file-loader进行copy。
```

```javascript
// webpack.config.js 
rules:[
    {
        test:/\.(png|svg|jpg|gif|jpeg)$/i,
        type:'asset/resource'
    }
]
```

```javascript
// 如果使用webpack5，仍然使用fileloader和url-loader
rules:[
    {
      test:/\.(png|svg|jpg|gif|jpeg)$/i,
      use:{
      	loader:'url-loader',
        options:{
            limit:5000
        }
      }
    },
    type:'javascript/auto'
]
```

##### 管理资源对比

> 在` webpack 5` 之前，通常使用：
>
> - [`raw-loader`](https://webpack.docschina.org/loaders/raw-loader/) 将文件导入为字符串
> - [`url-loader`](https://webpack.docschina.org/loaders/url-loader/) 将文件作为 data URI 内联到 bundle 中
> - [`file-loader`](https://webpack.docschina.org/loaders/file-loader/) 将文件发送到输出目录
>
> 资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：
>
> - `asset/resource` 发送一个单独的文件并导出 URL。之前通过使用 `file-loader` 实现。
> - `asset/inline` 导出一个资源的 data URI。之前通过使用 `url-loader` 实现。
> - `asset/source` 导出资源的源代码。之前通过使用 `raw-loader` 实现。
> - `asset` 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 `url-loader`，并且配置资源体积限制实现。

自定义输出文件名默认情况下，`asset/resource` 模块以 `[hash][ext][query]` 文件名发送到输出目录。
可以通过在 `webpack `配置中设置 `output.assetModuleFilename` 来修改此模板字符串：

```javascript
// 两种方式  注意：asset/resource
// 第一种
 output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
    assetModuleFilename:'images/[hash]1111[ext][query]'
  }
// 第二种
{
    test: /\.(png|jpg)$/i,
    type:'asset/resource',
    generator:{
      filename:'static/[hash][ext][query]'
    }
}
```

##### 加载字体

```javascript
{
    test: /\.(woff|woff2|eot|ttf|otf)$/i,
    type: 'asset/resource',
    // 可以自定义输出别名
}
```

```css
/* 引入字体,style.css;将style.css引入到入口文件 */
@font-face{
    font-family: 'Mango';
    src: url('./Mango.otf');
    font-weight: 600;
    font-style: normal;
}
.hello {
    font-family: 'Mango';
}
```

##### 加载数据

```javascript
//  json是内置支持的，不需要额外配置，xml和csv，利用xml-loader和csv-loader
//  自定义json的配置，请参考webpack管理资源文档
```

#### 管理输出

***当我们修改了入口文件，或者增加了一个新入口，就得相应的修改dist/index.html,HtmlWebpackPlugin就是用来解决这个问题的***

```javascript
// 看一个例子
entry: {
     index: './src/index.js',
     print: './src/print.js',
},
output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
    assetModuleFilename:'images/[hash]1111[ext][query]'
 },
 // 会生成index.bundle.js  和 print.bundle.js   都需要引入dist/index.html
```

`安装HtmlWebpackPlugin`

```javascript
npm install --save-dev html-webpack-plugin
// 配置
const HtmlWebpackPlugin = require('html-webpack-plugin')
plugins:[new HtmlWebpackPlugin({title:'管理输出'})]
```

`清理dist文件夹`

```javascript
//  在每次构建前清理 /dist 文件夹
//  npm install --save-dev clean-webpack-plugin
```

```javascript
npm install --save-dev clean-webpack-plugin
// 配置
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
plugins:[
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
       title: 'Output Management',
     })
]
```

#### 开发环境

```javascript
{
    mode:'development',
    devtool:'inline-map'
}
```

监听文件变化，只介绍`webpack-dev-server`,其余方式请查看开发环境文档
安装

```javascript
npm install --save-dev webpack-dev-server
```

配置

```javascript
devServer: {
   contentBase: './dist',
}
// package.json配置
{
    "serve":"webpack serve --open"
}
```

使用 `webpack-dev-middleware`
***`webpack-dev-middleware` 是一个封装器(wrapper)，它可以把 `webpack` 处理过的文件发送到一个 server。 ***
参考文档
#### 代码分离

[`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin) 插件可以将公共的依赖模块提取到已有的入口 chunk 中，或者提取到一个新生成的 chunk

```javascript
optimization: {
  splitChunks: {
    chunks: 'all',
  },
},
```

#### 缓存

- 输出文件的文件名：我们可以通过替换 output.filename 中的 substitutions 设置，来定义输出文件的名称。webpack 提供了一种使用称为 substitution(可替换模板字符串) 的方式，通过带括号字符串来模板化文件名。其中，[contenthash] substitution 将根据资源内容创建出唯一 hash。当资源内容发生变化时，[contenthash] 也会发生变化。

- 提取引导模板：SplitChunksPlugin 可以用于将模块分离到单独的 bundle 中。webpack 还提供了一个优化功能，可使用 optimization.runtimeChunk 选项将 runtime 代码拆分为一个单独的 chunk。将其设置为 single 来为所有 chunk 创建一个 runtime bundle：

  ```javascript
  optimization: {
     runtimeChunk: 'single',
  },
  ```

- 模块标识符:

  ```javascript
  optimization:{
      moduleIds：'deterministic',
      runtimeChunk:'single',
      splitChunks:{
          cacheGroups:{
              vendor:{
                  test:/[\\/]node_modules[\\/]/,
                  name: 'vendors',
             	    chunks: 'all',
              }
          }
      }
  }
  ```

#### 创建library 

```javascript
// 配置项很简单
{
    entry: './src/index.js',
   output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'webpack-numbers.js',
    library: 'webpackNumbers',   //暴露出的变量名称
    libraryTarget: 'umd',   // 查看文档，umd格式可以用于script标签
    //有以下几种方式暴露 library：

	//  变量：作为一个全局变量，通过 script 标签来访问（libraryTarget:'var'）。
    //  this：通过 this 对象访问（libraryTarget:'this'）。
    //  window：在浏览器中通过 window 对象访问（libraryTarget:'window'）。
    //  UMD：在 AMD 或 CommonJS require 之后可访问（libraryTarget:'umd'）。
   },
   // 排除不想打入的包
   externals: {
    lodash: {
      commonjs: "lodash",
      commonjs2: "lodash",
      amd: "lodash",
      root: "_",
    },
  },
}
```

#### 环境变量

```javascript
npx webpack --env NODE_ENV=local --progress
// or
npx webpack --env production --progress
// webpack.config.js
module.exports = env=>{
    console.log("NODE_ENV",env.NODE_ENV) // local
    console.log("Production: ", env.production); // true
}
```















