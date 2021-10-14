### 一、项目初始化

1. npm init -y

- 初始化工程生成 package.json

2. npm install webpack webpack-cli -D

- 安装 webpack -D: 安装开发环境依赖

### 二、编写项目入口

1. 编写 index.html

- 写上 id 为'app'的 div 标签

2. 编写 vue 根实例

- 在 main.js 中,创建 vue 跟实例、挂载 app 组件

  1). 安装 vue

- npm install vue

  2). 创建 Vue 根实例挂载组件

  ```javascript
  new Vue({
    el: "app",
    components: {
      //组件名： 组件对象
      App: App,
    },
    template: "<App/>",
  });
  ```

  3). 在 index.html 引入 main.js,发现需要打包

  ### 三、webpack 配置

  1. 新建 webpack.config.js

```javascript
//model.export{
    //打包入口
    //打包出口
}
```

2. 在 package.json 设置

```javascript
"scripts": {
        "build": "webpack"
    },
}
```

### 四、使用 vue-loader 打包 vue 文件

1. 安装插件

```javascript
npm install vue-loader vue-template-compiler -D
npm install -D css-loader
```

2. webpack 配置

```javascript
// 使用node的path模块
const path = require("path");

// 引入vue-loader插件
const VueLoaderPlugin = require("vue-loader/lib/plugin");

module.exports = {
  //模式
  mode: "production",
  // 打包的入口
  entry: "./src/main.js",
  // 打包的出口
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  // 打包规则
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: "vue-loader",
      },
    ],
  },
  plugins: [
    // 请确保引入这个插件！
    new VueLoaderPlugin(),
  ],
};
```

3. webpack.config.js 在配置中添加 mode：'development'

4. 在 index.html 中引入 bundle.js

- Vue 会打包生成三个文件:
  1). runtime only 的文件 vue.common.js
  2).compiler only 的文件 compiler.js
  3).runtime + compiler 的文件 vue.js
- 默认导入第一个需要 webpack 中配置

```javascript
resolve: {
    alias: {
      'vue': 'vue/dist/vue.js'
    }
  },
```

5. 总结

- webpack 本身只能打包 js 文件，如果要打包其他文件就需要 loader
- loader 其实就是专门用于特定文件的处理程序
- 打包是把多个 js 文件打包成一个减少请求，提升相应效率

### 五、其他常用 loader

1. 打包图片文件

```javascript
npm i file-loader -D

//如果是图片则用file-loader打包
{
    test: /\.(jpg|jpeg|png|svg)$/,
    loader: 'file-loader',
//打包图片自定义命名
    options: {
        name: '[name].[ext]'
    }
}
```

2. url-loader

3. css 打包

```javascript
npm i css-loader style-loader -D

//如果是图片则用file-loader打包
{
    test: /\.(jpg|jpeg|png|svg)$/,
    loader: 'file-loader',
//打包图片自定义命名
    options: {
        name: '[name].[ext]'
    }
}
```

- css-loader: 解决文件之间的依赖关系, 把所有的 css 文件打包成一个文件

- style-loader: 将 css-loader 打包完成后生成的文件挂载到页面的 head 标签的 style 中

3. 打包 stylus 文件

```javascript
npm i -D stylus stylus-loader

module: {
  rules: [{
    test: /\.styl(us)?$/,
    use: ['style-loader', 'css-loader', 'stylus-loader']
  }]
},
```

### 六、插件

1. 使用 html-webpack-plugin 插件

- 自动生成 index.html，把打包好的 js 文件引入进去

2. clean-webpack-plugin

- 自动删除源 js，然后生成打包

3. autoprefixer 插件

- css 加各个浏览器前缀 npm install -D postcss-loader autoprefixer postcss

### 七、开发环境

1. devServer

- webpack-dev-server 提供一个简单的 web 服务器，并且能够实时重新加载 保存时实时更新并自动重新打包

```javascript
npm i -D webpack-dev-server

// webpack.config.js  devServer配置
devServer: {
  // 指定服务器根目录
  contentBase: './dist',
  // 自动打开浏览器
  open: true
  // 启用热模块替换
  hot: true
},

//package.json

"start": "webpack-dev-server"
```

2. 热模块替换

```javascript
new webpack.HotModuleReplacementPlugin();
```

3. SourceMap

- 在开发时快速定位到出错的源代码行

### 七、生产环境

1. 分别指定两个配置文件 webpack.dev.js webpack.prod.js

```javascript
"scripts": {
        "dev": "webpack-dev-server --config ./webpack.dev.js",
        "build": "webpack --config ./webpack.prod.js"
    },
```

2. 提取公共部分

1) 安装 webpack-merge

2) 新建 webpack.base.js 提取公共部分，引入 dev,prod,中 merge 合并

### 八、解析 es6 语法

1. babel 是一个 javaScript 编译器 可以把 es6 转为 es5

```javascript
npm install -D balel-loader @balel/core
npm install @babel/preset-env -D

//添加打包规则
{ test: /\.js$/, exclude: /node_modules/, loader: 'babel-loader' }

//创建.babelrc 配置文件
{ "presets": ["@babel/preset-env"] }
```
