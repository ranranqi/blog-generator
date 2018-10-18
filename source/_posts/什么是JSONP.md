---
title: 什么是JSONP?
date: 2018-10-18 12:56:40
tags:
---
请求方：frank.com 的前端程序员（浏览器）
响应方：jack.com 的后端程序员（服务器）

1，请求方创建 script，src 指向响应方，同时传一个查询参数 ?callbackName=yyy
2，响应方根据查询参数callbackName，构造形如
  yyy.call(undefined, '你要的数据')
  或yyy('你要的数据')
这样的响应
3，浏览器接收到响应，就会执行 yyy.call(undefined, '你要的数据')
4，那么请求方就知道了他要的数据
这就是 JSONP
```
button.addEventListener('click', (e)=>{
    let script = document.createElement('script')
    let functionName = 'frank'+ parseInt(Math.random()*10000000 ,10)
    window[functionName] = function(){  // 每次请求之前搞出一个随机的函数
        amount.innerText = amount.innerText - 0 - 1
    }
    script.src = '/pay?callback=' + functionName
    document.body.appendChild(script)
    script.onload = function(e){ // 状态码是 200~299 则表示成功
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
    script.onload = function(e){ // 状态码大于等于 400 则表示失败
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
})
```

//后端代码
```
if (path === '/pay'){
    let amount = fs.readFileSync('./db', 'utf8')
    amount -= 1
    fs.writeFileSync('./db', amount)
    let callbackName = query.callback
    response.setHeader('Content-Type', 'application/javascript')
    response.write(`
        ${callbackName}.call(undefined, 'success')
    `)
    response.end()
}
```
约定：
callbackName -> callback
yyy -> 随机数 frank12312312312321325()



使用jQuery

```
 $.ajax({
 url: "http://jack.com:8002/pay",
 dataType: "jsonp",
 success: function( response ) {
     if(response === 'success'){
     amount.innerText = amount.innerText - 1
     }
 }
 })
 ```

为什么JSONP不支持POST？
1，因为JSONP是通过动态创建script实现的；
2，动态创建script只能用GET请求，没办法用POET。