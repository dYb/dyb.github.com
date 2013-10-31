---
layout: post
title: "front end workflow"
description: "前后端的工作配合流程，适用于RESTful API或后端java使用freemarker模板"
category: "workflow front-end"
tags: [workflow, front-end]
---
{% include JB/setup %}

2013_10_30

首先感谢[@ijse](http://www.ijser.cn/),这篇文章介绍的工作流程以及使用到的FED工具，都是他的工作成果，这里拾人牙慧整理下。

### 前后端的工作配合流程

正如上面所说，这些流程并非本人原创，但是使用起来会明显提高工作效率。**本文使用与RESTful API或者后端使用java并使用freemarker模板**，现在大多数web项目都是使用java吧~  

## 为什么会有这个流程

  之前，我们前后端配合是这样的，前端写静态的html页面，后端需要将html页面改成ftl模板，问题是在后端套页面时，经常会导致页面出现错乱，还是需要前端去调试，效率自然降低了。  
  此外，如果由后端写ajax的话会造成前端的代码混乱，在前端代码日益复杂的情况下，这显然不是好现象。  
  同时前端也需要自动打包部署工具，这里我们使用gruntjs完成一系列工作。

## 如何解决

首先，不要放弃治疗，虽然大家都习惯了这种合作方式，但不代表这样是合理的。  

正式开始介绍我们的解决方案：**前后端约定接口（也就是controler层的VO），前端直接写ftl模板，顺带模拟ajax后端返回值**，这样前后端的代码完全分开，后端不必套页面，只需要按事先约定的数据结构在controller层渲染相应的ftl模板  

这样做的好处显而易见：

*	前后端解耦，不必相互依赖
*	各司其职，后端不接触前端代码，前端不关心后端实现
*	明确需求，对接口的过程中，双方对需求都需要理解到位
*	一键部署，开发时可以使用coffee、less，而不需要手动编译

为了完成提到的实现方案，首先要有个解析ftl的工具，再次感谢[@ijes](http://www.ijser.cn/)的辛苦工作，开发了一个[FED](https://github.com/ijse/FED)工具，这是一个基于node实现的ftl解析工具，更多内容请移步[这里](https://github.com/ijse/FED/wiki)，有了它我们的问题就可以解决了。

## 使用步骤
具体可参考[这里](https://github.com/dYb/webstart)

1.	下载基础目录结构`git clone https://github.com/dYb/webstart.git`
2.	安装fed工具`npm install fed -g`
3.	启动服务`cd webstart && fed server -w ./fedConfig.js`
4.	访问 http://localhost:3000
5.  部署 `npm install & grunt dist`

文档结构看起来是这样子的  

*	`frontend`文件夹中放我们开发时的编码
*	`mocks`文件夹中放模拟后端提供的VO的数据结构
*	`fedConfig.js`是FED工具的配置文件
*	`webapp`是正式上线里的文件
*	`gruntfile.coffee`是grunt的配置文件，具体内容点[这里](https://github.com/dYb/webstart/blob/master/gruntfile.coffee)

## 注意事项

1. 前端所有的代码都位于 `frontend/src` 中，而发布的代码位于`webapp`中
2. 由于使用grunt编译并部署 `frontend/src`中的less、coffee和ftl模板，所以在webapp中的任何修改都会被覆盖，这样的好处就是，后端不能动前端代码
3. 提交到版本库里时，**必须运行`grunt dist`命令**，而开发中则不必修改，因为fed可以识别coffee和less等






