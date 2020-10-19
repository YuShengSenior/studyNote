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

# webpack.config.js  
作用: 指示webpack干哪些活,当webpack指令运行时,会加载里面的配置  
```js
const path = require('path')

module.exports = {
  // 入口文件
  entry: './src/index.js',
  // 输出
  output: {
    // 输出文件名
    filename: 'bundel.js',
    // __dirname nodejs变量,代表当前文件的目录的绝对路径
    path: path.resolve(__dirname, 'dist')
  },
  // loader配置
  module: {
    rules: [
    // 详细的loader配置
      {
        // 正则表达式,匹配以css结尾的
        test: /\.less$/,
        // 使用哪些loader处理
        use: [
          // use数组中执行顺序:从下到上
          // 创建style标签,将js的样式资源插入,添加到head中生效
          'style-loader',
          // 将css文件变成commonjs模块加载js中,内内容是样式字符串
          'css-loader',
          // 将less文件编译成css文件
          'less-loader'
        ]
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        // 依赖于file-loader
        loader: 'url-loader',
        options: {
          // 图片大小小于limit值进行base64处理
          limit: 8 * 1024,
          // 关闭es6模块化解析,使用CommonJS解析
          esModule: false,
          // 取图片的hash的前10位 取文件的原拓展名
          name: '[hash:10].[ext]'
        },
      },
      {
        test: /\.html$/,
        // 处理html文件的img标签图片(负责引入img,从而能被url-loader进行处理)
        loader: 'html-loader'
      }
    ],
    // plugins配置
    pugins:[
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
