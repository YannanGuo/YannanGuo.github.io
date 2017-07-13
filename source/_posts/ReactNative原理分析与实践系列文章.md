title: React Native 原理分析与实践系列文章
date: 2017-1-1 00:00:00
tags:
---

篇系列文章主要分析ReactNative源码，分析ReactNative的启动流程、渲染原理、通信机制与线程模型等方面内容。

<!-- more -->

# React Native

<img src="https://github.com/guoxiaoxing/awesome-react-native/raw/master/art/react_native_banner.png" width="1000"/>

**关于作者**

>郭孝星，非著名程序员，主要从事Android平台基础架构与中间件方面的工作，爱好广泛，技术栈主要涉及以下几个方面
>
>- Android/Linux
>- Java/Kotlin/JVM
>- Python
>- JavaScript/React/ReactNative
>- DataStructure/Algorithm
>
>文章首发于[Github](https://github.com/guoxiaoxing)，后续也会同步在[简书](http://www.jianshu.com/users/66a47e04215b/latest_articles)与
[CSDN](http://blog.csdn.net/allenwells)等博客平台上。文章中如果有什么问题，欢迎发邮件与我交流，邮件可发至guoxiaoxingse@163.com。


文章持续更新中...

## ReactNative基础篇

>本篇系列文章主要介绍ReactNative的基本组件以及一些基本用法。

- [1ReactNative实践篇：环境配置与编译](https://github.com/guoxiaoxing/react-native/blob/master/doc/ReactNative基础篇/1ReactNative实践篇：环境配置与编译.md)
- 2ReactNative实践篇：基本概念
- 3ReactNative实践篇：基本组件-View
- 4ReactNative实践篇：基本组件-Text、TextInput与Button
- 5ReactNative实践篇：基本组件-ScrollView、ListView
- 6ReactNative实践篇：基本组件-MapView
- 7ReactNative实践篇：基本组件-WebView
- 8ReactNative实践篇：基本组件-Modal
- 9ReactNative实践篇：基本组件-Navigator
- 10ReactNative实践篇：基本组件-Picker
- 11ReactNative实践篇：基本组件-Slider
- 12ReactNative实践篇：基本组件-Switch
- 13ReactNative实践篇：基本组件-Touchable系列
- 14ReactNative实践篇：基本组件-DrawerLayoutAndroid
- 15ReactNative实践篇：基本组件-ProgressBarAndroid
- 16ReactNative实践篇：基本组件-ToolbarAndroid
- 17ReactNative实践篇：基本组件-ViewPagerAndroid
- 18ReactNative实践篇：网络请求
- 19ReactNative实践篇：网页浏览器、音视频媒体播放

## ReactNative源码篇

>本篇系列文章主要分析ReactNative源码，分析ReactNative的启动流程、渲染原理、通信机制与线程模型等方面内容。

- [1ReactNative源码篇：源码初识](https://github.com/guoxiaoxing/react-native/blob/master/doc/ReactNative源码篇/1ReactNative源码篇：源码初识.md)
- [2ReactNative源码篇：代码调用](https://github.com/guoxiaoxing/react-native/blob/master/doc/ReactNative源码篇/2ReactNative源码篇：代码调用.md)
- [3ReactNative源码篇：启动流程](https://github.com/guoxiaoxing/react-native/blob/master/doc/ReactNative源码篇/3ReactNative源码篇：启动流程.md)
- [4ReactNative源码篇：渲染原理](https://github.com/guoxiaoxing/react-native/blob/master/doc/ReactNative源码篇/4ReactNative源码篇：渲染原理.md)
- [5ReactNative源码篇：线程模型](https://github.com/guoxiaoxing/react-native/blob/master/doc/ReactNative源码篇/5ReactNative源码篇：线程模型.md)
- [6ReactNative源码篇：通信机制](https://github.com/guoxiaoxing/react-native/blob/master/doc/ReactNative源码篇/6ReactNative源码篇：通信机制.md)
					
## ReactNative实践篇

>本篇系列文章主要从性能优化、崩溃监测等方面进一步阐述ReactNative在生产环境下的最佳实践以及改进优化。

- 1ReactNative实践篇：性能优化
- 2ReactNative实践篇：崩溃监测


## 附录

### React Native优秀资料

推荐一些关于React Native的比较高质量资料。

- [Awesome React Native](https://github.com/jondot/awesome-react-native)：Awesome React Native components, news, tools, and learning material!
- [react-native-guide](https://github.com/reactnativecn/react-native-guide)：React Native指南汇集了各类react-native学习资源、开源App和组件
- [fangwei716/30-days-of-react-native](https://github.com/fangwei716/30-days-of-react-native)
- [fbsamples/f8app](https://github.com/fbsamples/f8app)
- [构建 F8 App / React Native 开发指南](https://f8-app.liaohuqiu.net/)

### React Native技术分享

前段时间在公司分享会上做了关于React Native实现原理的分享，以下是分享会的PPT。

<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片01.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片02.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片03.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片04.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片05.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片06.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片07.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片08.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片09.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片10.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片11.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片12.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片13.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片14.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片15.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片16.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片17.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片18.png"/>
<img src="https://github.com/guoxiaoxing/react-native/raw/master/art/source/ppt/幻灯片19.png"/>
