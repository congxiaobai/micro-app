<p align="center">
  <a href="https://zeroing.jd.com/micro-app/">
    <img src="https://cangdu.org/micro-app/_media/logo.png" alt="logo" width="180"/>
  </a>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@micro-zoe/micro-app">
    <img src="https://img.shields.io/npm/v/@micro-zoe/micro-app.svg" alt="version"/>
  </a>
  <a href="https://www.npmjs.com/package/@micro-zoe/micro-app">
    <img src="https://img.shields.io/npm/dt/@micro-zoe/micro-app.svg" alt="downloads"/>
  </a>
  <a href="https://github.com/micro-zoe/micro-app/blob/master/LICENSE">
    <img src="https://img.shields.io/npm/l/@micro-zoe/micro-app.svg" alt="license"/>
  </a>
  <a href="https://gitter.im/zoe-community/zoe-room">
    <img src="https://badges.gitter.im/Join%20Chat.svg" alt="gitter">
  </a>
  <a href="https://travis-ci.com/github/bailicangdu/micro-app">
    <img src="https://travis-ci.com/bailicangdu/micro-app.svg?branch=master" alt="travis"/>
  </a>
  <a href="https://coveralls.io/github/bailicangdu/micro-app?branch=master">
    <img src="https://coveralls.io/repos/github/bailicangdu/micro-app/badge.svg?branch=master" alt="coveralls"/>
  </a>
</p>

[Discussions](https://github.com/micro-zoe/micro-app/discussions)

# 📖简介
[micro-app](https://github.com/micro-zoe/micro-app/issues/8)是一款基于类WebComponent进行渲染的微前端框架，不同于目前流行的开源框架，它从组件化的思维实现微前端，旨在降低上手难度、提升工作效率。它是目前市面上接入微前端成本最低的框架，并且提供了JS沙箱、样式隔离、元素隔离、预加载、资源地址补全、插件系统、数据通信等一系列完善的功能。

micro-app与技术栈无关，也不和业务绑定，可以用于任何前端框架和业务。

#### 概念图
![image](https://img10.360buyimg.com/imagetools/jfs/t1/168885/23/20790/54203/6084d445E0c9ec00e/d879637b4bb34253.png ':size=750')

# 🔧开始使用
微前端分为基座应用和子应用，我们分别列出基座应用和子应用需要进行的修改，具体介绍micro-app的使用方式。

`下述以react代码为例`

### 基座应用
1、安装依赖
```bash
yarn add @micro-zoe/micro-app
```

2、在入口处引入依赖
```js
// index.js
import microApp from '@micro-zoe/micro-app'

microApp.start()
```

3、分配一个路由给子应用
```js
import { BrowserRouter, Switch, Route } from 'react-router-dom'
import MyPage from './my-page'

export default function AppRoute () {
  return (
    <BrowserRouter>
      <Switch>
        // 👇 非严格匹配，/my-page/* 都将匹配到 MyPage 组件
        <Route path='/my-page'>
          <MyPage />
        </Route>
        ...
      </Switch>
    </BrowserRouter>
  )
}
```

4、在页面中使用组件
```js
// my-page.js
export function MyPage () {
  return (
    <div>
      <h1>加载子应用</h1>
      /**
       * 1、micro-app为自定义标签，可以在任何地方使用
       * 2、name为应用名称，全局唯一
       * 3、url为html地址
       */
      <micro-app name='app1' url='http://localhost:3000/' baseurl='/my-page'></micro-app>
    </div>
  )
}
```

### 子应用
添加路由前缀

```js
import { BrowserRouter, Switch, Route } from 'react-router-dom'

export default function AppRoute () {
  return (
    // 👇 添加路由前缀，子应用可以通过window.__MICRO_APP_BASE_URL__获取基座下发的baseurl
    <BrowserRouter basename={window.__MICRO_APP_BASE_URL__ || '/'}>
      <Switch>
        ...
      </Switch>
    </BrowserRouter>
  )
}
```
以上即完成了微前端的渲染。

> 注意
>
> 1、子应用需要支持跨域访问
>
> 2、url属性指向的是html地址，不会影响子应用，子应用的路由是基于浏览器地址进行匹配的，详情请查看[路由一章](https://zeroing.jd.com/micro-app/docs.html#/zh-cn/route)

**在线案例**：https://zeroing.jd.com/micro-app/demo/

# 🤝 参与共建
如果你对这个项目感兴趣，欢迎提PR参与共建，也欢迎您 "Star" 支持一下 ^_^

### 本地运行
1、下载项目
```
git clone https://github.com/micro-zoe/micro-app.git
```

2、安装依赖
```
yarn bootstrap
```

3、运行项目
```
yarn start # 访问 http://localhost:3000
```

默认启动react基座应用，如果想启动vue基座应用，可以运行`yarn start:main-vue2`

#### 你也可以使用 Gitpod 进行在线开发：
<a href="https://gitpod.io/#https://github.com/micro-zoe/micro-app">
  <img src="https://cangdu.org/img/open-in-gitpod.svg" alt="gitpod"/>
</a>

# 🤔常见问题
[问题汇总](https://zeroing.jd.com/micro-app/docs.html#/zh-cn/questions)
<details>

  <summary>micro-app的优势在哪里？</summary>
  上手简单、功能强大。

  具体细节请参考文章：[micro-app介绍](https://github.com/micro-zoe/micro-app/issues/8)

</details>
<details>
  <summary>兼容性如何？</summary>
  micro-app依赖于CustomElements和Proxy两个较新的API。

  对于不支持CustomElements的浏览器，可以通过引入polyfill进行兼容，详情可参考：[webcomponents/polyfills](https://github.com/webcomponents/polyfills/tree/master/packages/custom-elements)。

  但是Proxy暂时没有做兼容，所以对于不支持Proxy的浏览器无法运行micro-app。

  浏览器兼容性可以查看：[Can I Use](https://caniuse.com/?search=Proxy)

  总体如下：
  - PC端：除了IE浏览器，其它浏览器基本兼容。
  - 移动端：ios10+、android5+
</details>

<details>
  <summary>子应用一定要支持跨域吗？</summary>
  是的！

  如果是开发环境，可以在webpack-dev-server中设置headers支持跨域。
  ```js
  devServer: {
    headers: {
      'Access-Control-Allow-Origin': '*',
    },
  },
  ```

  如果是线上环境，可以通过[配置nginx](https://segmentfault.com/a/1190000012550346)支持跨域。
</details>

<details>
  <summary>支持vite吗?</summary>
  
  支持，详情请查看[适配vite](https://zeroing.jd.com/micro-app/docs.html#/zh-cn/other?id=_3%e3%80%81%e9%80%82%e9%85%8dvite)
</details>

# License
micro-app base on **MIT** license
