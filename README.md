# Webpack 搭建 ES6 开发环境

## 安装模块

### 创建 package.json 文件
```bash
npm init -y
```

### 安装 webpack
```bash
npm install webpack webpack-cli --save-dev
```

### 安装 babel
```bash
npm install babel-loader @babel/core @babel/preset-env --save-dev
```

### 安装 CSS 加载器
```bash
npm install css-loader style-loader --save-dev
```

### 安装 HTML 插件
```bash
npm install html-webpack-plugin --save-dev
```

## 创建目录结构

### 项目目录结构介绍
```
- project [ 根目录 ]
​ – node_modules [ 项目中的依赖存放目录 ]
​ – public [ 静态资源文件目录 ]
​ – src [ 项目源文件目录 ]
​ – dist [ 打包文件目录 ]
​ – webpack.config.js [ webpack配置文件 ]
​ – package.json [ NPM包管理配置文件 ]
 - ...
 - ...
```

### public 目录

在 public 目录下，创建 index.html，该文件为项目的默认首页。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

### src 目录
在 src 目录下，创建 index.js，该文件为项目的入口文件，在此文件中可以编写 ES6 代码。

### webpack.config.js 文件

在项目的根目录下创建 webpack.config.js 配置文件，依次完成以下配置：

- 配置入口（entry）
  ```js
  module.exports = {
      entry: './src/index.js'
  }
  ```

- 配置出口（output）
  ```js
  const path = require('path');
  module.exports = {
      //...
      output: {
          path: path.resolve(__dirname, 'dist'),
          filename: 'main.js'
      }
  }
  ```
  
- 配置加载器（loader）
  ```js
  module.exports = {
    module: {
      rules: [
        {
          test: /\.css$/,
          use: ['style-loader', 'css-loader']
        },
        {
          test: /\.js?$/, // js 或 jsx 文件的正则
          exclude: /node_modules/, // 排除 node_modules 文件夹
          use: {
            // loader 是 babel
            loader: 'babel-loader',
            options: {
              // babel 转义的配置选项
              babelrc: false,
              preset: [
                [require.resolve('@babel/preset-env'), {modules: false}]
              ],
              cacheDirectory: true
            }
          }
        }
      ]
    }
  }
  ```  
  
- 配置插件（plugin）
  ```js
  const HtmlWebPackPlugin = require('html-webpack-plugin');
  module.exports = {
  	// ...
  	plugins:[
  		new HtmlWebPackPlugin({
  			template: 'public/index.html',
  			filename: 'index.html',
  			inject: true
  		})
  	]
  }
  ```

## 搭建本地服务

### 安装依赖
```bash
npm install webpack-dev-server --save-dev
```

### webpack.config.js 文件中配置
```js
module.exports = {
	// ...
	devServer: {
		contentBase: './dist',
		host: 'localhost',
		port: 8888
	}
}
```

### package.json 文件中配置启动命令

```js
{
    "scripts": {
        "start": "webpack-dev-server --mode development --open"
    }
}
```

### 启动服务
```
npm start
```

## 打包

### package.json 文件中配置打包命令
```js
{
    "scripts": {
        "build": "webpack --mode production"
    }
}
```

### 启动打包功能
```
npm build
```




