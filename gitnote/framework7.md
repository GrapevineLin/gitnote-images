**网上关于Framework7的博客、学习资料少之又少，所以我想把我学习Framework7 Vue的入门记录一下。**

> Framework7 是一个开源免费的框架可以用来开发混合移动应用（原生和HTML混合）或者开发 iOS & Android 风格的WEB APP。也可以用来作为原型开发工具，可以迅速创建一个应用的原型。
Framework7 最主要的功能是可以使用HTML、CSS和JS来开发iOS7应用。Framework7 是完全免费开源的。
Framework7 并不能兼容所有的设备。她只专注于为 iOS 和 Google Material 设计提供最好的体验。
**如果你想开发 iOS 或者 Android 混合应用（Phonegap）或者你想开发 iOS 和 Google Material 风格的WEB APP，那么Framework7将会是你的首选。**

首先我们进入Framework7的文档官网，https://framework7.io/vue/installation.html，注意英文文档才是最新的，中文文档则是很久没更新的旧版本。



我们采用手动安装（Manual Installation）的方式，首先你的电脑要有vue+webpack的开发环境，然后依次安装framework7和framework7-vue相关依赖，最后修改一下文件结构即可。

初始化一个vue应用
vue init webpack framework7-vue-demo


安装framework7和framework7-vue
npm install framework7
npm install framework7-vue

修改vue文件结构（初始化App）
 官网文档 Initialize App 这一节中的 ES Modules 有相应的指导，我们要修改的文件有index.html、main.js(my-app.js)、  app.vue。 

 首先是index.html，官网给的是这样子的



 经实践发现这样子会在chrome移动端调试的时候出现缩放问题，所以我们这样子改：



<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="Content-Security-Policy" content="default-src * 'self' 'unsafe-inline' 'unsafe-eval' data: gap: content:">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no, minimal-ui, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <meta name="theme-color" content="#2196f3">
  <meta name="format-detection" content="telephone=no">
  <meta name="msapplication-tap-highlight" content="no">
  <title> framework7-vue </title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
 main.js：
import Vue from 'vue'
import Framework7 from 'framework7/framework7.esm.bundle.js'
import Framework7Vue from 'framework7-vue/framework7-vue.esm.bundle.js'
import Framework7Theme from 'framework7/css/framework7.bundle.css'

Framework7.use(Framework7Vue);
import App from './app.vue';
new Vue({
  el: '#app',
  render: (h) => h(App),
});


 比官网给的多了一行导入 Framework7Theme  ，如果没有这个导入将会没有样式效果

 app.vue：
<template>
  <f7-app :params="f7params">
    <f7-view main url="/"></f7-view>
  </f7-app>
</template>
<script>
  import routes from './router/index.js';
  export default {
    data() {
      return {
        f7params: {
          routes:routes,
          name: 'My App',
          id: 'com.myapp.test',
          theme: 'auto'
        }
      }
    }
  }
</script>
 跟官网给的有一处不同即routes的导入，这个根据实际路由文件导入就好了，另外路由文件也和原vue的有所不同，查看文档的 Navigation / Router 这一节，我们将路由文件改为：

import HelloWorld from '@/components/HelloWorld'
export default [

  {
    path:'/',
    component:HelloWorld
  }
];

 为了查看效果，我找了文档中的一个实例 Tabbar 的实例代码替换入helleworld.vue：

<template>
  <f7-page :page-content="false">
    <f7-navbar title="Tabbar" back-link="Back">
      <f7-nav-right>
        <f7-link icon-ios="f7:reload" icon-md="material:compare_arrows" @click="isBottom = !isBottom"></f7-link>
      </f7-nav-right>
    </f7-navbar>
    <f7-toolbar tabbar :position="isBottom ? 'bottom' : 'top'">
      <f7-link tab-link="#tab-1" tab-link-active>Tab 1</f7-link>
      <f7-link tab-link="#tab-2">Tab 2</f7-link>
      <f7-link tab-link="#tab-3">Tab 3</f7-link>
    </f7-toolbar>

    <f7-tabs>
      <f7-tab id="tab-1" class="page-content" tab-active>
        <f7-block>
          <p>Tab 1 content</p>
          ...
        </f7-block>
      </f7-tab>
      <f7-tab id="tab-2" class="page-content">
        <f7-block>
          <p>Tab 2 content</p>
          ...
        </f7-block>
      </f7-tab>
      <f7-tab id="tab-3" class="page-content">
        <f7-block>
          <p>Tab 3 content</p>
          ...
        </f7-block>
      </f7-tab>
    </f7-tabs>
  </f7-page>
</template>
<script>
  export default {
    data() {
      return {
        isBottom: true,
      };
    }
  }
</script>
 

可以看到framework7是正常起作用的。

至此，就可以开始学习使用framework7了。


