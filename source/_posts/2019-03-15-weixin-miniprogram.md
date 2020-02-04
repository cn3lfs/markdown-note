---
title: weixin_miniprogram
author: bro wen
date: 2019-03-15 18:14:54
tags: [miniprogram]
---
# 微信小程序

## 总体介绍
小程序跟SPA有点像，JS core + webview渲染层，只是小程序js逻辑和渲染逻辑是分开的，而SPA中是交替执行的。

JSON 文件里面是用来配置小程序的，这也是非常熟悉的，但是里面只能用双引号，跟package.json有点类似。

## wxml
wxml 里面标签必须要严格闭合，否则会报错。
WXML中的属性是大小写敏感的。

### 数据绑定

小程序里面的数据绑定跟Vue有点类似
{{var}} 对应 data 里面的 var
but 区分大小写
没有被定义的变量的或者是被设置为 undefined 的变量不会被同步到 wxml 中{}里面可以放简单的 expression

### Conditional Rendering

类似 Vue 有 v-if v-else-if -v-else ，小程序里面有 wx:if wx:elif wx:else
同样 vue 切换多个对象在template 上处理，小程序用block

### List Rendering

Vue: v-for="item in items" :key="item.id"     小程序: wx:for="{{something}}"

注意小程序中默认每个元素就是item 索引是 index
使用 wx:for-item 指定数组当前元素的变量名，使用 wx:for-index 指定数组当前下标的变量名

类似 block wx:if ，也可以将 wx:for 用在 <block/>标签上，以渲染一个包含多节点的结构块

如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如 <input/> 中的输入内容， <switch/> 的选中状态），需要使用 wx:key 来指定列表中项目的唯一的标识符。

wx:key 的值以两种形式提供：

    字符串，代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变。这里直接就是写成wx:key="property"而不是wx:key="item.property"

    保留关键字 this 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字，如：

当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。

这跟Vue 里面的 Patch 模式有点类似,key提供给Vue用于追踪每个element,使得这个过程可以充分reuse existing elements.

### template

WXML提供模板（template），可以在模板中定义代码片段，然后在不同的地方调用。使用 name 属性，作为模板的名字。然后在 <template/> 内定义代码片段，使用 is 属性，声明需要的使用的模板，然后将模板所需要的 data 传入.is里面加一个tenary逻辑判断,可以动态决定具体需要渲染哪个模板.

### import and include

WXML 提供两种文件引用方式import和include。

需要注意的是 import 有作用域的概念，即只会 import 目标文件中定义的 template，而不会 import 目标文件中 import 的 template，简言之就是 import 不具有递归的特性。
include 可以将目标文件中除了template外的整个代码引入，相当于是拷贝到 include 位置.

### public attr

表2-1 共同属性
属性名     类型  描述  注解
id  String  组件的唯一标识     整个页面唯一
class   String  组件的样式类  在对应的 WXSS 中定义的样式类
style   String  组件的内联样式     可以动态设置的内联样式
hidden  Boolean     组件是否显示  所有组件默认显示
data-*  Any     自定义属性   组件上触发的事件时，会发送给事件处理函数
bind*/catch*    EventHandler    组件的事件