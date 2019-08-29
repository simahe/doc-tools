```
npm config set prefix "D:\Program Files\nodejs\node_global"
npm config set cache "D:\Program Files\nodejs\node_cache"
进入环境变量对话框，在【系统变量】下新建【NODE_PATH】，输入【D:\Program Files\nodejs\node_modules】，
将【用户变量】下的【Path】修改为【D:\Program Files\nodejs\node_global】
```



```
npm list --depth 0
```

```
##### 使用create-react-app创建react项目：

​```
$ npm install create-react-app -g
$ create-react-app  one
$ cd one
$ npm install
$ npm start
​```

- 要打包到生产环境的安装包时，你应该使用 `npm install --save`
- 用于开发环境的安装包（例如，linter, 测试库等），使用 `npm install --save-dev`
```

# NPM基本操作

```
$ npm -v
$ sudo npm install npm -g   升级
$ npm init  *********创建模块
$ npm install npm -g
$ npm install express      # 本地安装
$ npm install express -g   # 全局安装
$ npm config set proxy null
$ npm list -g   查看安装信息
$ npm list grunt 如果要查看某个模块的版本
$ npm list --depth 0  查看安装信息
$ npm uninstall express 卸载模块
$ npm ls 命令查看
$ npm update express 更新模块
$ npm search express 搜索模块
```







```
npm i --save-dev cross-env       cross-env能跨平台地设置及使用环境变量
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```