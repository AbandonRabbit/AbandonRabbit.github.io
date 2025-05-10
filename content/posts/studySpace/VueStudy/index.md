---
date: '2025-05-09T18:14:49+08:00'
draft: false
title: 'Vue'
seriesOpened: false #s是否开启系列
# series: [""] #属于的系列 
# series_order: 0  #系列编号
tags: ["Vue"]
Categories: ["学习笔记"]
layoutBackgroundBlur: false #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---
# 环境搭建

1. 安装 [Node.js](https://nodejs.org/zh-cn)，并配置环境变量。--这一步会自动安装node.js 和 npm 包管理器
2. 使用 `node -v` 和 `npm -v`检测是否安装成功
3. 使用 `npm init vue@latest` 创建项目
4. npm config set registry https://registry.npmmirror.com

node_modules  Vue项目的运行依赖文件夹
public  资源文件夹(浏览器图标)
src 源码文件夹
index.html  如果HTML文件
package.json  信息描述文件
vite.config.js  Vue配置文件



Vue.js是典型的MVVM框架





文本绑定 {{}}

属性绑定 v-bind

# 条件渲染

v-if  v-else  v-if-else

v-if 如果为 true 显示

v-else else的块

v-if-else 接下来判断的块  决定是否显示

v-show 

区别  v-show 只能判断自身是否显示，v-if 可以添加多个条件

v-if 时控制元素是否存在，v-show时控制元素是否显示，性能开销主要体现在什么时候渲染和渲染的次数上

v-for

v-for="(item,index) in List"

在methods里面添加方法  

computed 计算属性 在里面写一些属性值的返回

watch 监听数据变化 里面的方法第一个参数为new 第二个参数为old    方法名必须与监听的数据对象保持一致

components 引入的页面

props 接受父组件传递过来的值

