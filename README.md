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

### 第4章:准备工作

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
    // 这里开始配置，before函数为提供的生命钩子，app即devserver的实例
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
### 第5章:v-Router

1. 怎么配置默认路由？怎么调整vue-router的api？

答：用redirect。同时可以应用vue-router的api，例如修改激活时的class name

```javascript
export default new Router({
  linkActiveClass: 'active', // 自定义激活的classname
  routes: [
    {
      path: '/',
      redirect: '/goods' // 重定向实现默认路由
    },
    {
      path: '/goods',
      name: 'Goods',
      component: goods
    },
    {
      path: '/ratings',
      name: 'Satings',
      component: ratings
    },
    {
      path: '/seller',
      name: 'Seller',
      component: seller
    }
  ]
})
```

2. 怎么实现移动端的1px？

答：首先用伪类实现1px的像素，然后通过-webkit-min-device-pixel-ratio与min-device-pixel-ratio媒询设备的DPR，对元素边框进行具体缩放。

用伪类生成边框
```stylus
border-1px($color)
  position relative
  &:after
    display block
    position absolute
    left 0
    bottom: 0
    width: 100%
    border-top: 1px solid $color
    content: ' '
```

全局定义样式 媒询设备的DPR
```stylus
@media (-webkit-min-device-pixel-ratio:1.5),(min-device-pixel-ratio:1.5)
  .border-1px
    &::after
        -webkit-transform: scaleY(0.7)
        transform: scaleY(0.7)
@media (-webkit-min-device-pixel-ratio:2),(min-device-pixel-ratio:2)
  .border-1px
    &::after
        -webkit-transform: scaleY(0.5)
        transform: scaleY(0.5)
```
### 第6章:header组件

1. CSS sticky footer

[科普文章](https://www.w3cplus.com/css3/css-secrets/sticky-footers.html)

html布局
```html
<div class="detail-wrapper clearfix">
  <div class="detail-main">
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
    <p>{{seller.bulletin}}</p>
  </div>
</div>
<div class="detail-close">
  <i class="icon-close"></i>
</div>
```

CSS布局
```stylus
.detail-wrapper
  min-height 100%
  .detail-main
    margin-top 64px
    padding-bottom 64px
.detail-close
  position relative
  width 32px
  height 32px
  margin -64px auto 0 auto
  clear both
  font-size 32px
```
