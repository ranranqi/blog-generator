---
title: 实现一个jQuery的API
date: 2018-10-08 22:27:38
tags: jQuery
---
```
window.jQuery = ???
window.$ = jQuery

var $div = $('div')
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi
```

jQuery不仅仅可以实现传入一个node，还可以传入选择器去找到对应的元素。
然后做类型检测看传入的参数是字符串还是节点
```
window.jQuery = function(nodeOrSelector){     
  let node
  if(typeof nodeOrSelector === 'string'){         
    node = document.querySelector(nodeOrSelector)
  }else(nodeOrSelector instanceof Node){
    node = nodeOrSelector
    }
  return node
}
```
如果jQuery调用的是字符串，就根据这个字符串找到对应的节点，node就保存这个节点。

如果想同时操作几个div，给所有div的class添加一个red
```
window.jQuery = function(nodeOrSelector){
  let nodes = {}             //所有获取到的node
  if(typeof nodeOrSelector === 'string'){  
    let temp = document.querySelectorAll(nodeOrSelector)       //返回一个伪数组，做一个临时变量temp
    for(let i=0; i<temp.length; i++){          //遍历这个temp，得到一个没有NodeList的原型链。放在nodes里。
      nodes[i] = temp[i]
    }
    nodes.length = temp.length
  }else if(nodeOrSelector instanceof Node){    // 也返回一个伪数组
    nodes ={
      0: nodeOrSelector,
      length: 1
    }
  }
  nodes.addClass = function(classes){
    classes.forEach((value) => {
      nodes[0].classList.add(value)
      nodes[1].classList.add(value)
      nodes[2].classList.add(value)
      nodes[3].classList.add(value)
      nodes[4].classList.add(value)
    })
  }
  return nodes
}

var $div = $('div')
$div.addClass('red')

```
但是我们不可能获取多少个就去添加多少个，这样很麻烦代码量也很大，所以可以通过for循环遍历这个nodes。
```
nodes.addClass = function(classes){
    classes.forEach((value) => {
      for(let i=0; i<nodes.length; i++>){
          nodes[i].classList.add(value)
      }
    })
  }
```

jQuery接受一个字符串'div'，然后去做类型检测看是字符串还是节点，如果是字符串就去找到对应的所有元素放到一个伪数组里面，如果是个节点，也放在一个伪数组里面。
也就是说不管得到一个什么，都会找到并放在一个伪数组里面。
然后通过`$div.addClass('red')`调用。实际上通过forEach遍历这个classes，在伪数组的每一项元素里执行一个函数，给每一项添加一个class。

让所有 div 的 textContent 变为 hi
```
nodes.setText = function(text){
    for(let i=0;i<nodes.length;i++){
      nodes[i].textContent = text
    }
  }
```
遍历nodes，把每一项里面的内容变成 hi
然后通过 `$div.setText('hi')`调用。

完整代码
```
window.jQuery = function(nodeOrSelector){
  let nodes = {}
  if(typeof nodeOrSelector === 'string'){
    let temp = document.querySelectorAll(nodeOrSelector)
    for(let i=0; i<temp.length; i++){
      nodes[i] = temp[i]
    }
    nodes.length = temp.length
  }else if(nodeOrSelector instanceof Node){
    nodes ={
      0: nodeOrSelector,
      length: 1
    }
  }
  
  
  nodes.addClass = function(classes){
    classes.forEach((value) => {
      for(let i=0;i<nodes.length;i++){
        nodes[i].classList.add(value)
      }
    })
  }
  nodes.setText = function(text){
    for(let i=0; i<nodes.lnength; i++){
      nodes[i].textContent = text
   }
  }
  
  
  return nodes
}


var $div = $('div')

$div.addClass('red')
$div.setText('hi')
```