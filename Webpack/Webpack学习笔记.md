Webpack是什么?
webpack是前端资源构建工具,是一个静态模块打包器  
# Entry  
入口指示,webpack以哪个文件为入口起点开始打包,分析构建内部依赖图
# Output
输出指示,webpack打包后的资源bundles输出到哪里去,以及如何命名
# Loader
Loader让webpack能够处理那些非JS文件,例如图片,sass,less等文件
# Plugins
插件可以用于执行范围更广的任务, 插件的范围包括,从打包优化和压缩一直到重新定义环境变量
# Mode
一般分为两种,开发模式和生产模式  
| 选项 | 描述 | 特点 |  
|:---:|:---:|:---|
|development|会将process.env.NODE_ENV的值设置为development <br> 启用NamedChunksPlugin 和 NameModulesPlugin|能让代码本地调试运行的环境|
|production|会将process.env.NODE_ENV的值设置为production<br>启用FlagDependencyUsagePlugin,<br>FlagIncludedChunksPlugin<br>ModuleConcatenationPlugin,<br>NoEmitOnErroesPlugin,<br>OccurrenceOrderPlugin,<br>SideEffectsFlagPlugin和UglifyjsPlugin|能让代码优化上线运行的环境|  
### 生产与开发环境对比  
- webpack本身能处理js,json资源,不能处理css,img等其他资源  
- 生产环境比开发环境多一个压缩js代码的功能
- 生产环境和开发环境能够将es6+的语法模块编译成浏览器可以识别的模块
# webpack.config.js  
作用: 指示webpack干哪些活,当webpack指令运行时,会加载里面的配置  
```js
const path = require('path')

module.exports = {
  entry: './src/index.js',   // 入口文件
  output: {   // 输出
    filename: 'bundel.js',     // 输出文件名
    path: path.resolve(__dirname, 'dist'),     // __dirname nodejs变量,代表当前文件的目录的绝对路径
    publicPath: './' 
    /** 
     * ⚠️ webpack5.1.x以上的版本需要配置这个
     * 让html中img标签路径正确,因为输出的是build文件夹,但是不会创建新的文件夹
     * 如狗哦我们将要输出的资源放在build中其他的文件夹,则我们的./也要改成相对路径,需要与publicPath一致
    */
  },
  // loader配置
  module: {
    rules: [ // 详细的loader配置
      {
        test: /\.less$/, // 正则表达式,匹配以less结尾的
        // 使用哪些loader处理
        use: [ // use数组中执行顺序:从下到上
          'style-loader', // 创建style标签,将js的样式资源插入,添加到head中生效
          'css-loader', // 将css文件变成commonjs模块加载js中,内内容是样式字符串
          'less-loader' // 将less文件编译成css文件
        ]
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        loader: 'url-loader',  // 依赖于file-loader
        options: {
          limit: 8 * 1024, // 图片大小小于limit值进行base64处理
          // esModule: false, // 关闭es6模块化解析,使用CommonJS解析
          // ⚠️  !5.1.x版本已经修复这个问题了,不需要对这个配置,
          name: '[hash:10].[ext]' // 取图片的hash的前10位 取文件的原拓展名
        },
      },
      {
        test: /\.html$/, // 处理html文件的img标签图片(负责引入img,从而能被url-loader进行处理)
        loader: 'html-loader'
      }
    ],
    pugins:[ // plugins配置
      // 详细的plugins配置
      new HtmlWebpackPlugin(
        // 复制./src/index/html文件,引入打包输出的所有资源(js/css)
        template: './src/index/html'
      )
      // 功能: 默认会常见一个空的html,引入打包输出的所有资源(js/css)
    ]
  },
  // 模式
  mode:'development'
  // 或者为 production
};
```
## 开发环境配置  
开发环境能让代码运行即可
```js
/**
 * 开发环境配置
 */

const { resolve } = require("upath");
const HtmlWebpackPlugin = require("html-webpack-plugin")

module.exports = {

  entry: './src/index.js',
  output: {
    filename: 'js/built.js',
    path: resolve(__dirname, 'build')
  },
  module: {
    rules: [
      // less
      {
        test: /\.less$/,
        use: [
          'style-loader',
          'css-loader',
          'less-loader'
        ]
      },
      // css
      {
        test: /\css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      },
      // 图片
      {
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          name: '[hash:10],[ext]',
          esModule: false,
          outputPath: 'imgs'
        },
      },
      // html 中的图片
      {
        test: /\.html$/,
        loader: 'html-loader'
      },
      {
        exclude: /\.(.html|js|css|less|jpg|png|gif)$/,
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]',
          outputPath: 'media'
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  mode: 'development',
  devServer: {
    contentBase: resolve(__dirnanme, 'build'),
    compress: true,
    prot: 3000,
    open: true
  }

}
```


