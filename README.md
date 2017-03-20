#react配置
###1. 新建项目结构

###2. 创建package.json文件
> npm init

###3. 安装webpack
>npm i webpack --save-dev

###4. 创建webpack.config.js

```
 //__dirname是node.js中的一个全局变量，它指向当前执行脚本所在的目录
module.exports = {
    entry: __dirname + "/app/main.js",//唯一入口文件，就像Java中的main方法
    output: {//输出目录
        path: __dirname + "/build",//打包后的js文件存放的地方
        filename: "bundle.js"//打包后的js文件名
    }
};
```

###5. 更新package.json文件，更方便的打包
将npm的start命令指向webpack命令

npm的start命令是一个特殊的脚本名称，若配置其它名称，需npm **run**  名称

```
{
  "name": "react0",
  "version": "1.0.0",
  "description": "从0开始构建react项目",
  "main": "index.js",
  "scripts": {
    "start": "webpack"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/hangview/react0.git"
  },
  "author": "hangview",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/hangview/react0/issues"
  },
  "homepage": "https://github.com/hangview/react0#readme",
  "devDependencies": {
    "react": "^15.4.2",
    "react-dom": "^15.4.2"
  }
}
```

###6. Source Maps 方便调试

```
module.exports = {
    devtool:'source-map',   //生成source maps，方便代码调试
    entry: __dirname + "/app/main.js",
    output: {
        path: __dirname + "/build",
        filename: "bundle.js"
    }
};
```

###7. 安装React

> npm i --save-dev react react-dom

###8. 安装babel

babel有两个用途

* 支持新一代javascript标准  es6
*  支持基于javascript扩展的语言，如JSX

Npm一次性安装所有babel相关的包:

> npm install --save-dev babel-core babel-loader  babel-preset-es2015 babel-preset-react

配置

1. 在webpack配置loader加载器

```

module.exports = {
    devtool:'source-map',
    entry: __dirname + "/app/main.js",
    output: {//输出目录
        path: __dirname + "/build",
        filename: "bundle.js"
    },
    module: {
        loaders:[{
            test: /\.(js|jsx)$/,   //匹配loader所处理文件拓展名（必须）
            exclude: /node_modules/, //屏蔽不需处理的文件（可选）
            loader:'babel'  //loader的名称（必须）
            }
        ]

    }
};
```

2. 提取babel相关配置到独立文件 .babelrc

```
//.babelrc
{
  "presets": [
    "react",
    "es2015"
  ]
}
```

###9. 安装配置webpack-dev-server

写好react组件和配置好入口文件后，配置webpack-dev-server

> npm i --save-dev webpack-dev-server

配置：

1. 在webpack.config.js文件中配置webpack-dev-server

```
var webpack = require('webpack');//引入Webpack模块供我们调用，这里只能使用ES5语法，使用ES6语法会报错


module.exports = {//注意这里是exports不是export
    devtool:'source-map',  //生成source maps，方便代码调试
    entry: __dirname + "/app/main.jsx",//唯一入口文件，就像Java中的main方法

    ....


    plugins: [
        new webpack.HotModuleReplacementPlugin()  //热模块替换插件
    ],

    //webpack-dev-server 配置
    devServer: {
        contentBase: './build',//默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到"build"目录）
        colors: true,//在cmd终端中输出彩色日志
        historyApiFallback: true,//在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html
        inline: true,//设置为true，当源文件改变时会自动刷新页面
        port: 3001,//设置默认监听端口，如果省略，默认为"8080"
        process: true//显示合并代码进度
    }
};
```

2.  在package.json文件中添加webpack热加载命令

>   "scripts": {
    "start": "webpack",
    "dev": "webpack-dev-server"
  }



 10. 注意

 配置时发现webpack,webpack-dev-server版本问题，成功版本：

>     "webpack": "^2.1.0-beta.21",
    "webpack-dev-server": "^2.1.0-beta.9"