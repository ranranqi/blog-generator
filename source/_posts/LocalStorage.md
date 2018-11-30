---
title: LocalStorage
date: 2018-11-30 14:46:00
tags: http
---
LocalStorage是Html5提供的一个API，也可以说是浏览器中的一个hsah表。用来保存浏览器中的所有变量。
在没有LocalStorage以前，页面刷新之后所有的变量全都会销毁，没办法保留下来，但是有了LocalStorage我们就可以把所有的东西存下来，用来在以后使用。LocalStorage也称持久化存储。LocalStorage保存在C盘的一个文件里。

LocalStorage的几个方法：
如：`setItem()`, `getItem()`, `clear()`等.
用getItem setItem来操作

设值：
`localStorage.setItem("hello", "world");`
`localStorage.setItem("zhangsan", "lisi");`
取值：
`localStorage.getItem("hello");`
`localStorage.getItem("zhangsan");`

LocalStorage的特点：
1，LocalStorage与HTTP无关。
2，HTTP不会带上LocalStorage的值到服务器上去。
3，只有相同域名的页面才能互相读取LocalStorage。
4，每个域名的LocalStorage最大存储量只有5MB左右，但浏览器不一样。
5，常用场景：记录有么有提示过用户
6，LocalStorage永久有效，除非用户自己清理。

另： sessionStorage 也是HTML5新增的一个会话存储对象，用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。
在JavaScript语言中可通过 window.sessionStorage 或 sessionStorage 调用此对象。
也有LocalStorage的`setItem()`, `getItem()`等方法。

sessionStorage的特点：
同LocalStorage特点的1，2，3，4。
唯一不同的是sessionStorage在页面关闭之后就失效。