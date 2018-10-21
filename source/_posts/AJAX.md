---
title: AJAX
date: 2018-10-21 16:41:28
tags: ajax
---
### 先了解AJAX之前发送请求的几种方法。（src="/xxx"）
1，用form表单发送请求，会刷新页面或新开页面；
2，用a标签发送请求，但也会刷新页面或新开页面；
3，用img标签可以发送请求，只能以图片形式展示；
4，用link可以发送请求，只能以css,favicon形式展示；
5，用script发送请求，例如：JSONP，但只能以脚本形式执行。
以上发送请求的方式只能用 "GET"，有限制性。所以就出现了使用 AJAX发请求。
AJAX不仅可是使用GET，还可以使用POST，PUT，DELETE等动词形式，还可以以不同的形式展示。
### AJAX（Asynchronous Javascript And XML）
即异步的Javascript和Xml。
使用AJAX满足三个要求：
1，使用XMLHttpResquest发请求；
2，服务器返回XML格式的字符串；
3，JS解析XML，并更新局部页面。

手写一个AJAX，只需8行代码:
```
let request = new XMLHttpRequest()
  request.open('get', '/xxx') // 配置request
  request.send()
  request.onreadystatechange = ()=>{
    if(request.readyState === 4){
      if(request.status >= 200 && request.status < 300){
        let string = request.responseText
        let object = window.JSON.parse(string)
      }
    }
  }
```


### 如何使用XMLHttpResquest
使用XMLHttpResquest发送请求最重要的一句：
```
let request = new XMLHttpResquest()
```

把XMLHttpResquest通过new变成一个对象；

第二句配置request：
```
request.open(method,url)
```
其中open中写http的请求方法和路径
method 的值包括GET，POST，PUT，DELETE等。

第三句监听readyState的状态变化
```
request.onreadystatechange = ()=>{
    if(request.readyState === 4){
        console.log('请求响应都完毕了')
    }
}
```
第四句
```
requset.send()
```
代码：
```
myButton.addEventListener('click', (e)=>{
  let request = new XMLHttpRequest()
  request.open('get', '/xxx') // 配置request
  request.send()
  request.onreadystatechange = ()=>{
    if(request.readyState === 4){
      console.log('请求响应都完毕了')
      console.log(request.status)
      if(request.status >= 200 && request.status < 300){
        console.log('说明请求成功')
        console.log(typeof request.responseText)
        console.log(request.responseText)
        let string = request.responseText
        // 把符合 JSON 语法的字符串
        // 转换成 JS 对应的值
        let object = window.JSON.parse(string) 
        // JSON.parse 是浏览器提供的
        console.log(typeof object)
        console.log(object)
        console.log('object.note')
        console.log(object.note)

      }else if(request.status >= 400){
        console.log('说明请求失败') 
      }

    }
  }
})
```
上述代码把服务器返回的XML格式的字符串转换成了JSON格式，因为Xml格式的文件过于麻烦，所以使用了JSON格式来代替它。
后端代码
```
}else if(path==='/xxx'){
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/json;charset=utf-8')
    response.setHeader('Access-Control-Allow-Origin', 'http://frank.com:8001')
    response.write(`
    {
      "note":{
        "to": "小谷",
        "from": "方方",               //JSON格式
        "heading": "打招呼",
        "content": "hi"
      }
    }
    `)
    response.end()
```

AJAX最重要的一点： **只有 “ 协议 + 端口 + 域名 ”一模一样才允许发送AJAX请求！！！**

### 同源策略
因为同源策略的存在，所以只有你的域名端口协议一模一样才能发送这个请求，如果没有就毫无隐私可言。所有浏览器必须保证只有域名端口协议一模一样才允许发送AJAX请求。

突破同源策略 === 跨域

### CORS(Cross-Origin Resource Sharing)跨域资源共享
原理：服务器设置Access-Control-Allow-Origin HTTP响应头之后，浏览器才会允许跨域请求。

使用AJAX发送请求，如果想再在xxx.com下访问yyy.com 。域名不一样就无法访问，但是可以通过CORS实现跨域请求。
在yyy.com的后端代码加上一句：
```
response.setHeader('Access-Control-Allow-Origin','http://xxx.com:8001')
```
这样xxx.com就能访问yyy.com.实现跨域。就是告诉浏览器这两个是一家的不要阻止访问。