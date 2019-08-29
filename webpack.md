webpack文档地址

来源 https://www.webpackjs.com

- 入口(entry)  webpack 使用哪个模块，来作为构建其内部*依赖图*的开始
- 输出(output) 
- 装载(loader)   将所有类型的文件，转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。
- 插件(plugins)  从打包优化和压缩，一直到重新定义环境中的变量。用来处理各种各样的任务





```
https://www.webpackjs.com/guides/getting-started/
```

##### 使用create-react-app创建react项目：

```
npm install create-react-app -g
create-react-app  project-name
cd project-name
npm start
```

- 要打包到生产环境的安装包时，你应该使用 `npm install --save`     or   -S
- 用于开发环境的安装包（例如，linter, 测试库等），使用 `npm install --save-dev`  or  -D



- chunk 数据块

- vendor 供应商 ( 第三方库)

- loader 装载机  leider  louder  

- *bundler*  打包机  
- *module*   摸丢 嘛紧
- plugins  插件

- compiler  编译器




# 1、起步

##### 基本安装

```
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```

##### 创建一个 bundle 文件

```
npm install --save lodash       通过打包来合成脚本
npx webpack                    默认输出 为 main.js，会将我们的脚本作为入口起点

```

##### 模块

##### 使用一个配置文件,手动添加**webpack.config.js**文件

```diff
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
**********************************
npx webpack --config webpack.config.js  现在，让我们通过新配置文件再次执行构建：
我们在这里使用 --config 选项只是向你表明，可以传递任何名称的配置文件。这对于需要拆分成多个文件的复杂配置是非常有用。 
```

##### NPM 脚本(NPM Scripts)

****

```diff
package.json文件设置
"scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "build": "webpack"
},
    
然后可以使用 npm run build   
```

##### 结论

```diff
webpack-demo
|- package.json
|- webpack.config.js
|- /dist
  |- bundle.js
  |- index.html
|- /src
  |- index.js
|- /node_modules
```

# 2、管理资源

```bash
npm install --save-dev style-loader css-loader     //样式
npm install --save-dev file-loader				//字体，图片等
npm install --save-dev csv-loader xml-loader     // CSV、TSV 和 XML

index.js 引入资源，webpack.config.js》module-rules 添加配置
```

# 3、管理输出

```
module.exports = {
    entry: {
        app: './src/index.js',
        print: './src/print.js'
    },
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist')
    },

};
```

##### 设定 HtmlWebpackPlugin

```bash
npm install --save-dev html-webpack-plugin  //安装插件,生成全新index.html文件

new HtmlWebpackPlugin({
      title: 'simahe',
      template: 'config/template.html'
      }),
```

##### 清理 `/dist` 文件夹

```bash
npm install clean-webpack-plugin --save-dev
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
```

##### Manifest

```
npm install  webpack-manifest-plugin --save-dev
npm uninstall  webpack-manifest-plugin --save-dev
```



# 4、开发

##### 使用 source map

****

```diff
module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
+   devtool: 'inline-source-map',
    plugins: [
     new CleanWebpackPlugin(),
    ],
    
最新版本 devtool: 'cheap-module-eval-source-map',
```

##### 选择一个开发工具

```diff
  //使用观察模式，要自己手动刷新浏览器
 "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "watch": "webpack --watch",
      "build": "webpack"
    },
    
使用 webpack-dev-server，自动刷新浏览器
npm install --save-dev webpack-dev-server 
设置"start": "webpack-dev-server --open",
npm start
```

##### 使用 webpack-dev-middleware *********************express server

```
webpack-dev-middleware 是一个容器(wrapper)，它可以把 webpack 处理后的文件传递给一个服务器(server)。 webpack-dev-server 在内部使用了它，同时，它也可以作为一个单独的包来使用，以便进行更多自定义设置来实现更多的需求。接下来是一个 webpack-dev-middleware 配合 express server 的示例。

首先，安装 express 和 webpack-dev-middleware：
npm install --save-dev express webpack-dev-middleware
 "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "watch": "webpack --watch",
      "start": "webpack-dev-server --open",
      "starton": "webpack-dev-server  --config  webpack.config.js --open",
+     "server": "node server.js",
      "build": "webpack"
    },
npm run server
```

# 5、模块热替换

##### 启用 HMR，它允许在运行时更新各种模块，而无需进行完全刷新

```diff
const webpack = require('webpack');
 const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const CleanWebpackPlugin = require('clean-webpack-plugin');
+ const webpack = require('webpack');

  module.exports = {
    entry: {
-      app: './src/index.js',
-      print: './src/print.js'
+      app: './src/index.js'
    },
    devtool: 'inline-source-map',
    devServer: {
      contentBase: './dist',
      contentBase: path.join(__dirname, "../dist"),
      compress: true,
      inline: true,
      disableHostCheck: true,
      host: 'localhost',
      port: 8282,
      overlay: {
      errors: true,
      }
+     hot: true
    },
    plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        title: 'Hot Module Replacement'
      }),
+     new webpack.NamedModulesPlugin(),    /*以便更容易查看要修补(patch)的依赖*/
+     new webpack.HotModuleReplacementPlugin()
    ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
  
  不启用的时候，修改会重新刷新页面，启用之后不会
```

# tree shaking

移除 JavaScript 上下文中的未引用代码(dead-code)

##### 添加一个通用模块

```javascript
export function square(x) {
  return x * x;
}
export function cube(x) {
  return x * x * x;
}
```

##### 将文件标记为无副作用(side-effect-free)

```json
这种方式是通过 package.json 的 "sideEffects" 属性来实现的。
{
  "name": "your-project",
  "sideEffects": false
}

module.rules: [
  {
    include: path.resolve("node_modules", "lodash"),
    sideEffects: false
  }
]
```

##### 压缩输出

```diff
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
- }
+ },
+ mode: "production"
};
注意，也可以在命令行接口中使用 --optimize-minimize 标记，来使用 UglifyJSPlugin。 
$ npm install uglifyjs-webpack-plugin --save-dev
```

##### 结论

- ```
  为了学会使用 *tree shaking*，你必须……
  - 使用 ES2015 模块语法（即 `import` 和 `export`）。
  - 在项目 `package.json` 文件中，添加一个 "sideEffects" 入口。
  - 引入一个能够删除未引用代码(dead code)的压缩工具(minifier)（例如 `UglifyJSPlugin`）。
  ```

# 生产环境构建

关注更小的 bundle，更轻量的 source map，以及更优化的资源，以改善加载时间。由于要遵循逻辑分离，我们通常建议为每个环境编写**彼此独立的 webpack 配置**。

我们还是会遵循不重复原则，保留一个“通用”配置。为了将这些配置合并在一起，我们将使用一个名为 [`webpack-merge`](https://github.com/survivejs/webpack-merge) 的工具。

```bash
npm install --save-dev webpack-merge

- |- webpack.config.js
+ |- webpack.common.js
+ |- webpack.dev.js
+ |- webpack.prod.js
```

##### NPM Scripts

```diff
  package.json
  "scripts": {
-     "start": "webpack-dev-server --open",
+     "start": "webpack-dev-server --open --config webpack.dev.js",
-     "build": "webpack"
+     "build": "webpack --config webpack.prod.js"
    },
    

代码压缩
虽然 UglifyJSPlugin 是代码压缩方面比较好的选择，但是还有一些其他可选择项。以下有几个同样很受欢迎的插件：BabelMinifyWebpackPlugin，ClosureCompilerPlugin
source map
鼓励你在生产环境中启用 source map
webpack.prod.js
```

7种SourceMap模式

开发环境推荐：cheap-module-eval-source-map
生产环境推荐：cheap-module-source-map

| 模式                    | 解释                                                         |
| ----------------------- | ------------------------------------------------------------ |
| eval                    | 每个module会封装到 eval 里包裹起来执行，并且会在末尾追加注释 `//@ sourceURL`. |
| source-map              | 生成一个**SourceMap**文件.                                   |
| hidden-source-map       | 和 source-map 一样，但不会在 bundle 末尾追加注释.            |
| inline-source-map       | 生成一个 **DataUrl** 形式的 SourceMap 文件.                  |
| eval-source-map         | 每个module会通过eval()来执行，并且生成一个DataUrl形式的SourceMap. |
| cheap-source-map        | 生成一个没有列信息（column-mappings）的SourceMaps文件，不包含loader的 sourcemap（譬如 babel 的 sourcemap） |
| cheap-module-source-map | 生成一个没有列信息（column-mappings）的SourceMaps文件，同时 loader 的 sourcemap 也被简化为只包含对应行的。 |





##### 指定环境

许多 library 将通过与 `process.env.NODE_ENV` 环境变量关联，以决定 library 中应该引用哪些内容。例如，当不处于*生产环境*中时，某些 library 为了使调试变得容易，可能会添加额外的日志记录(log)和测试(test)。其实，当使用 `process.env.NODE_ENV === 'production'` 时，一些 library 可能针对具体用户的环境进行代码优化，从而删除或添加一些重要代码。我们可以使用 webpack 内置的 [`DefinePlugin`](https://www.webpackjs.com/plugins/define-plugin) 为所有的依赖定义这个变量：

**webpack.prod.js**

```diff
+ const webpack = require('webpack');
  const merge = require('webpack-merge');
  const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
  const common = require('./webpack.common.js');

  module.exports = merge(common, {
    devtool: 'source-map',
    plugins: [
      new UglifyJSPlugin({
        sourceMap: true
-     })
+     }),
+     new webpack.DefinePlugin({
+       'process.env.NODE_ENV': JSON.stringify('production')
+     })
    ]
  });
  
src/index.js
+ if (process.env.NODE_ENV !== 'production') {
+   console.log('Looks like we are in development mode!');
+ }
```

##### Split CSS

正如在**管理资源**中最后的 [加载 CSS](https://www.webpackjs.com/guides/asset-management#loading-css) 小节中所提到的，通常最好的做法是使用 `ExtractTextPlugin` 将 CSS 分离成单独的文件。在插件[文档](https://www.webpackjs.com/plugins/extract-text-webpack-plugin)中有一些很好的实现例子。`disable` 选项可以和 `--env` 标记结合使用，以允许在开发中进行内联加载，推荐用于热模块替换和构建速度。

```
npm install --save-dev mini-css-extract-plugin

const MiniCssExtractPlugin = require('mini-css-extract-plugin');
```

##### CLI 替代选项

以上描述也可以通过命令行实现。例如，`--optimize-minimize` 标记将在后台引用 `UglifyJSPlugin`。和以上描述的 `DefinePlugin` 实例相同，`--define process.env.NODE_ENV="'production'"` 也会做同样的事情。并且，`webpack -p` 将自动地调用上述这些标记，从而调用需要引入的插件。





# 代码分离

把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件，代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。

有三种常用的代码分离方法：

- 入口起点：使用 [`entry`](https://www.webpackjs.com/configuration/entry-context) 配置手动地分离代码。
- 防止重复：使用 [`CommonsChunkPlugin`](https://www.webpackjs.com/plugins/commons-chunk-plugin) 去重和分离 chunk。
- 动态导入：通过模块的内联函数调用来分离代码。

##### 入口起点(entry points)

```diff
entry: {
    index: './src/index.js',
    another: './src/another-module.js'
  },
```

##### 防止重复(prevent duplication)

将公共的依赖模块提取到已有的入口 chunk 中，或者提取到一个新生成的 chunk。

```diff
webpack.config.js
 const path = require('path');
+ const webpack = require('webpack');
  module.exports = {
    plugins: [
      new HTMLWebpackPlugin({
        title: 'Code Splitting'
-     })
+     }),
+     new webpack.optimize.CommonsChunkPlugin({
+       name: 'common' // 指定公共 bundle 的名称。
+     })
    ],

***********************************************************
最新版本
const webpack = require('webpack');
//optimization与entry/plugins同级
optimization: {
        splitChunks: {
            cacheGroups: {
                commons: {
                    name: "commons",
                    chunks: "initial",
                    minChunks: 2
                }
            }
        }
    },

***********************************************************
以下是由社区提供的，一些对于代码分离很有帮助的插件和 loaders：

ExtractTextPlugin: 用于将 CSS 从主应用程序中分离。
bundle-loader: 用于分离代码和延迟加载生成的 bundle。
promise-loader: 类似于 bundle-loader ，但是使用的是 promises。

CommonsChunkPlugin 插件还可以通过使用显式的 vendor chunks 功能，从应用程序代码中分离 vendor 模块。
```

##### 动态导入(dynamic imports)

第一种，也是优先选择的方式是，使用符合 [ECMAScript 提案](https://github.com/tc39/proposal-dynamic-import) 的 [`import()` 语法](https://www.webpackjs.com/api/module-methods#import-)。第二种，则是使用 webpack 特定的 [`require.ensure`](https://www.webpackjs.com/api/module-methods#require-ensure)。让我们先尝试使用第一种……

```diff
- function getComponent() {
+ async function getComponent() {
-   return import(/* webpackChunkName: "lodash" */ 'lodash').then(_ => {
-     var element = document.createElement('div');
-
-     element.innerHTML = _.join(['Hello', 'webpack'], ' ');
-
-     return element;
-
-   }).catch(error => 'An error occurred while loading the component');
+   var element = document.createElement('div');
+   const _ = await import(/* webpackChunkName: "lodash" */ 'lodash');
+
+   element.innerHTML = _.join(['Hello', 'webpack'], ' ');
+
+   return element;
  }

  getComponent().then(component => {
    document.body.appendChild(component);
  });
```

##### bundle 分析(bundle analysis)

如果我们以分离代码作为开始，那么就以检查模块作为结束，分析输出结果是很有用处的。[官方分析工具](https://github.com/webpack/analyse) 是一个好的初始选择。下面是一些社区支持(community-supported)的可选工具：

- [webpack-chart](https://alexkuz.github.io/webpack-chart/): webpack 数据交互饼图。
- [webpack-visualizer](https://chrisbateman.github.io/webpack-visualizer/): 可视化并分析你的 bundle，检查哪些模块占用空间，哪些可能是重复使用的。
- [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer): 一款分析 bundle 内容的插件及 CLI 工具，以便捷的、交互式、可缩放的树状图形式展现给用户。

# 懒加载

懒加载或者按需加载，先把你的代码在一些逻辑断点处分离开，然后在一些代码块中完成某些操作，，立即引用或即将引用另外一些新的代码块。

###### 加快了应用的初始加载速度，减轻了它的总体体积，因为某些代码块可能永远不会被加载

```diff
 button.onclick = e => import(/* webpackChunkName: "print" */ './print').then(module => {
 var print = module.default;
 print();
});

框架

许多框架和类库对于如何用它们自己的方式来实现（懒加载）都有自己的建议。这里有一些例子：

React: Code Splitting and Lazy Loading
Vue: Lazy Load in Vue using Webpack's code splitting
AngularJS: AngularJS + Webpack = lazyLoad by @var_bincom

```

# 缓存

客户端（通常是浏览器）就能够访问网站此服务器的网站及其资源。而最后一步获取资源是比较耗费时间的，这就是为什么浏览器使用一种名为 [缓存](https://searchstorage.techtarget.com/definition/cache) 的技术。可以通过命中缓存，以降低网络流量，使网站加载速度更快

 webpack 编译生成的文件能够被客户端缓存，而在文件内容变化后，能够请求到新的文件。

##### 输出文件的文件名(Output Filenames)

```diff
output: {
-     filename: 'bundle.js',
+     filename: '[name].[chunkhash].js',
      path: path.resolve(__dirname, 'dist')
    }
```

##### 提取模板(Extracting Boilerplate)

就像我们之前从[代码分离](https://www.webpackjs.com/guides/code-splitting)了解到的，[`CommonsChunkPlugin`](https://www.webpackjs.com/plugins/commons-chunk-plugin) 可以用于将模块分离到单独的文件中。然而 `CommonsChunkPlugin` 有一个较少有人知道的功能是，能够在每次修改后的构建结果中，将 webpack 的样板(boilerplate)和 manifest 提取出来。通过指定 `entry` 配置中未用到的名称，此插件会自动将我们需要的内容提取到单独的包中：

**webpack.config.js**

```diff
+ const webpack = require('webpack');
+     new webpack.optimize.CommonsChunkPlugin({
+       name: 'manifest'
+     })


+   entry: {
+     main: './src/index.js',
+     vendor: [
+       'lodash'
+     ]
+   },

+     new webpack.optimize.CommonsChunkPlugin({
+       name: 'vendor'
+     }),
      new webpack.optimize.CommonsChunkPlugin({
        name: 'manifest'
      })
注意，引入顺序在这里很重要。CommonsChunkPlugin 的 'vendor' 实例，必须在 'manifest' 实例之前引入。 

new webpack.HashedModuleIdsPlugin(),
```

​              

# 创建 library

```diff
+  |- webpack.config.js
+  |- package.json
+  |- /src
+    |- index.js
+    |- ref.json


```

```bash
npm init -y
npm install --save-dev webpack lodash
```





# webpack4 创建react

```
  
npm install --save react react-dom  react-router-dom
npm install --save redux react-redux redux-thunk
npm install redux-devtools-extension -D
npm install --save redux-promise
```

```
npm install --save-dev @babel/preset-react
npm install --save-dev @babel/preset-flow  //类型注释 (Flow 和 TypeScript)
npm install --save-dev @babel/core @babel/cli @babel/preset-env
npm install --save-dev @babel/plugin-transform-arrow-functions //插件和预设（preset）
npm install --save @babel/polyfill


 {
 test: /\.(js|mjs|jsx|ts|tsx)$/,
 exclude: /(node_modules)/,
 use: {
 loader: 'babel-loader',
 options: {
 presets: ['@babel/preset-env']
 }
 }
 },

.babelrc
{
    "presets": [
        "@babel/preset-env", 
        "@babel/preset-react", 
        "mobx"
    ],
    "plugins": [
        "@babel/plugin-proposal-object-rest-spread",
        "@babel/plugin-transform-runtime"
    ]
}

babel.config.js


```

```
react-router 还是 react-router-dom？
在 React 的使用中，我们一般要引入两个包，react 和 react-dom，那么 react-router 和react-router-dom 是不是两个都要引用呢？
非也，坑就在这里。他们两个只要引用一个就行了，不同之处就是后者比前者多出了 <Link> <BrowserRouter> 这样的 DOM 类组件。
因此我们只需引用 react-router-dom 这个包就行了。当然，如果搭配 redux ，你还需要使用 react-router-redux。
what is the diff between react-router-dom & react-router?
```

css

```
npm install --save-dev style-loader css-loader     //样式
{
test: /\.css$/,
use: [
'style-loader',
'css-loader'
]
},
```

file

```
npm install --save-dev file-loader				//字体，图片等

{
test: /\.(png|svg|jpg|gif)$/,
use: [
'file-loader'
]
}
```

sass

```
npm install sass-loader node-sass webpack --save-dev

{
test: /\.scss$/,
use: [{
loader: "style-loader" // 将 JS 字符串生成为 style 节点
}, {
loader: "css-loader" // 将 CSS 转化成 CommonJS 模块
}, {
loader: "sass-loader" // 将 Sass 编译成 CSS
}]
},
```

postcss

```
npm install --save-dev  postcss-loader postcss-preset-env   autoprefixer precss 
npm install postcss-px-to-viewport --save-dev
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        exclude: /node_modules/,
        use: [
          {
            loader: 'style-loader',
          },
          {
            loader: 'css-loader',
            options: {
              importLoaders: 1,
            }
          },
          {
            loader: 'postcss-loader'
          }
        ]
      }
    ]
  }
}
后创建 postcss.config.js:

module.exports = {
  plugins: [
    require('precss'),
    require('autoprefixer')
  ]
}
```

fetch 

```
$ npm install node-fetch --save

```

```

```

## cross-env使用

运行[跨平台](https://www.baidu.com/s?wd=跨平台&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)设置和使用环境变量的脚本

```
npm install --save-dev cross-env

 "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server --open",
    "starton": "webpack-dev-server  --config  webpack.config.js --open",
    "dev:dev": "cross-env BUILD_ARGVS=dev NODE_ENV=dev webpack-dev-server --open",
    "dev:test": "cross-env BUILD_ARGVS=test NODE_ENV=test webpack-dev-server --open",
    "dev:pro": "cross-env BUILD_ARGVS=pro NODE_ENV=pro webpack-dev-server --config=webpack.config.js --open",
    "build:dev": "cross-env NODE_ENV=production webpack --config webpack.config.js",
    "build": "webpack"
  },
  
 一些说明 start 和 starton相同 默认  --config  webpack.config.js
 改变 webpack.config.js的位置名称  --config=config/webpack-dev.js
 cross-env 就可以设置参数了，但是要在webpack.config.js做一下设置
plugins: [
    new webpack.DefinePlugin({
      'process.env': {
           'BUILD_ARGVS': JSON.stringify(process.env.BUILD_ARGVS),
          'NODE_ENV': JSON.stringify(process.env.NODE_ENV),
      }
      }
  }),
  ],
然后你才能在工程js里面获取
const NODE_ENV = process.env.NODE_ENV;
const BUILD_ARGVS = process.env.BUILD_ARGVS;
```