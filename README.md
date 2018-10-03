# eleme

> 学习使我快乐之感谢滴滴前端大牛王老师的高仿eleme课程

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 笔记

### 第四章准备工作

1. 怎么在vue-cli 2.0中为webpack-dev-server加模拟api服务？

vuecli 2.0已经取消了dev-server.js的配置文件，node直接以webpack-dev-server命令启动依赖包来开启服务。devserver的配置整合进了webpack.dev.conf.js。

要增加配置，主要在devServer处设置：

``` javascript
const appData = require('../data.json')
const seller = appData.seller
const goods = appData.goods
const ratings = appData.ratings
...

const devWebpackConfig = merge(baseWebpackConfig, {
  module: {
    rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap, usePostCSS: true })
  },
  // cheap-module-eval-source-map is faster for development
  devtool: config.dev.devtool,

  // these devServer options should be customized in /config/index.js
  devServer: {
    before(app) {
      app.get('/api/seller', function(req,res) {
        res.json({
          error: 0,
          data: seller
        })
      });
      app.get('/api/goods', function(req,res) {
        res.json({
          error: 0,
          data: goods
        })
      });
      app.get('/api/ratings', function(req,res) {
        res.json({
          error: 0,
          data: ratings
        })
      });
    },
  ...
  }
...
}
```
