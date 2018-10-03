---
title: 关于DOM
date: 2018-10-03 16:08:23
tags:
---
# 1，什么是Dom ？
 全称(Document Object Model) “文档对象模型”，它是Javascript操作网页的接口，作用是将网页转化成一个Javascript对象，从而可以用脚本进行各种操作（如增删内容）。
 浏览器会根据Dom模型将结构化文档解析成一系列的节点，再由这些节点组成一个树状结构（Dom tree），所有节点和最终的树状结构都有规范的对外接口。
 Dom就是一个规接口规范，它可以由各种语言实现，但Dom操作是Javascript最常见的任务，离开了Dom，Javascript就无法控制网页。另一方面，Javascript也是最常用于Dom操作的语言。

 ## 2，节点（Node）
 是Dom最小的组成单位，各种不同类型的节点组成了文档的树形结构（Dom树），每一个节点都可以当作文档树的叶子。
 节点有七种类型，分别是：
* Document：整个文档树的顶层节点
* DocumentType：doctype标签（比如 !DOCTYPE html)
* Element：网页的各种HTML标签（比如 body、a等）
* Attribute：网页元素的属性（比如class="right"）
* Text：标签之间或标签包含的文本
* Comment：注释
* DocumentFragment：文档的片段
浏览器提供一个原生的节点对象Node，上面这七种节点都继承了Node，因此具有一些共同的属性和方法。

### 3，节点树
一个文档的所有节点，按照所在的层级，可以抽象成一种树状结构。这种树状结构就是 DOM 树。它有一个顶层节点，下一层都是顶层节点的子节点，然后子节点又有自己的子节点，就这样层层衍生出一个金字塔结构，倒过来就像一棵树。

浏览器原生提供document节点，代表整个文档。

document
`// 整个文档树`
文档的第一层只有一个节点，就是 HTML 网页的第一个标签 <html，它构成了树结构的根节点（root node），其他 HTML 标签节点都是它的下级节点。

除了根节点，其他节点都有三种层级关系。

父节点关系（parentNode）：直接的那个上级节点
子节点关系（childNodes）：直接的下级节点
同级节点关系（sibling）：拥有同一个父节点的节点
DOM 提供操作接口，用来获取这三种关系的节点。比如，子节点接口包括firstChild（第一个子节点）和lastChild（最后一个子节点）等属性，同级节点接口包括nextSibling（紧邻在后的那个同级节点）和previousSibling（紧邻在前的那个同级节点）属性。

#### 4，Javascript操作Dom常用API
##### 节点创建型api  
这一类型的API就是用来创建节点的。
**createElement**
createElement通过传入指定的一个标签名来创建一个元素，如果传入的标签名是一个未知的，则会创建一个自定义的标签，注意：IE8以下浏览器不支持自定义标签。
`var div = document.createElement("div");`
使用createElement要注意：通过createElement创建的元素并不属于html文档，它只是创建出来，并未添加到html文档中，要调用appendChild或insertBefore等方法将其添加到HTML文档树中。

**createTextNode**
createTextNode用来创建一个文本节点
`var textNode = document.createTextNode("一个TextNode");`
createTextNode接收一个参数，这个参数就是文本节点中的文本，和createElement一样，创建后的文本节点也只是独立的一个节点，同样需要appendChild将其添加到HTML文档树中
**cloneNode**
cloneNode是用来返回调用方法的节点的一个副本，它接收一个bool参数，用来表示是否复制子元素
**createDocumentFragment**
createDocumentFragment方法用来创建一个DocumentFragment。在前面我们说到DocumentFragment表示一种轻量级的文档，它的作用主要是存储临时的节点用来准备添加到文档中。
创建型api主要包括createElement，createTextNode，cloneNode和createDocumentFragment四个方法，需要注意下面几点：
（1）它们创建的节点只是一个孤立的节点，要通过appendChild添加到文档中
（2）cloneNode要注意如果被复制的节点是否包含子节点以及事件绑定等问题
（3）使用createDocumentFragment来解决添加大量节点时的性能问题

##### 页面修改型API
前面我们提到创建型api，它们只是创建节点，并没有真正修改到页面内容，而是要调用appendChild来将其添加到文档树中。我在这里将这类会修改到页面内容归为一类。
修改页面内容的api主要包括：appendChild，insertBefore，removeChild，replaceChild。
**appendChild**
appendChild我们在前面已经用到多次，就是将指定的节点添加到调用该方法的节点的子元素的末尾。调用方法如下：
`parent.appendChild(child);`
child节点将会作为parent节点的最后一个子节点。
appendChild这个方法很简单，但是还有有一点需要注意：如果被添加的节点是一个页面中存在的节点，则执行后这个节点将会添加到指定位置，其原本所在的位置将移除该节点，也就是说不会同时存在两个该节点在页面上，相当于把这个节点移动到另一个地方。
**insertBefore**
insertBefore用来添加一个节点到一个参照节点之前，用法如下：
`parentNode.insertBefore(newNode,refNode);`
parentNode表示新节点被添加后的父节点
newNode表示要添加的节点
refNode表示参照节点，新节点会添加到这个节点之前.

关于第二个参数参照节点还有几个注意的地方：
（1）refNode是必传的，如果不传该参数会报错
（2）如果refNode是undefined或null，则insertBefore会将节点添加到子元素的末尾
**removeChild**
removeChild顾名思义，就是删除指定的子节点并返回，用法如下：
`var deletedChild = parent.removeChild(node);`
deletedChild指向被删除节点的引用，它等于node，被删除的节点仍然存在于内存中，可以对其进行下一步操作。
注意：如果被删除的节点不是其子节点，则程序将会报错。我们可以通过下面的方式来确保可以删除：
```
if(node.parentNode){
     node.parentNode.removeChild(node);
}
```
通过节点自己获取节点的父节点，然后将自身删除。
**replaceChild**
replaceChild用于使用一个节点替换另一个节点，用法如下
`parent.replaceChild(newChild,oldChild);`
newChild是替换的节点，可以是新的节点，也可以是页面上的节点，如果是页面上的节点，则其将被转移到新的位置
oldChild是被替换的节点

页面修改型api主要是这四个接口，要注意几个特点：
（1）不管是新增还是替换节点，如果新增或替换的节点是原本存在页面上的，则其原来位置的节点将被移除，也就是说同一个节点不能存在于页面的多个位置
（2）节点本身绑定的事件会不会消失，会一直保留着。

##### 节点查询型API
节点查询型API也是非常常用的api，下面我们分别说明一下每一个api的使用。
**document.getElementById**
这个接口很简单，根据元素id返回元素，返回值是Element类型，如果不存在该元素，则返回null。
使用这个接口有几点要注意：
（1）元素的Id是大小写敏感的，一定要写对元素的id
（2）HTML文档中可能存在多个id相同的元素，则返回第一个元素
（3）只从文档中进行搜索元素，如果创建了一个元素并指定id，但并没有添加到文档中，则这个元素是不会被查找到的

**document.getElementsByTagName**
这个接口根据元素标签名获取元素，返回一个即时的HTMLCollection类型
HTMLCollcetion元素是即时的表示该集合是随时变化的，也就是是文档中有几个div，它会随时进行变化，当我们新增一个div后，再访问HTMLCollection时，就会包含这个新增的div。
使用document.getElementsByTagName这个方法有几点要注意：
（1）如果要对HTMLCollection集合进行循环操作，最好将其长度缓存起来，因为每次循环都会去计算长度，暂时缓存起来可以提高效率
（2）如果没有存在指定的标签，该接口返回的不是null，而是一个空的HTMLCollection
（3）“*”表示所有标签
**document.getElementsByName**
getElementsByName主要是通过指定的name属性来获取元素，它返回一个即时的NodeList对象。
使用这个接口主要要注意几点：
（1）返回对象是一个即时的NodeList，它是随时变化的
（2）在HTML元素中，并不是所有元素都有name属性，比如div是没有name属性的，但是如果强制设置div的name属性，它也是可以被查找到的
（3）在IE中，如果id设置成某个值，然后传入getElementsByName的参数值和id值一样，则这个元素是会被找到的，所以最好不好设置同样的值给id和name
**document.getElementsByClassName**
这个API是根据元素的class返回一个即时的HTMLCollection，用法如下
`var elements = document.getElementsByClassName(names);`
这个接口有下面几点要注意：
（1）返回结果是一个即时的HTMLCollection，会随时根据文档结构变化
（2）IE9以下浏览器不支持
（3）如果要获取2个以上classname，可传入多个classname，每个用空格相隔，例如
`var elements = document.getElementsByClassName("test1 test2");`
**document.querySelector**
接受一个选择器只返回选择器对应的函数。
**document.querySelectorAll**
接受一个选择器并返回选择器对应的所有函数。

##### 节点关系型api
在html文档中的每个节点之间的关系都可以看成是家谱关系，包含父子关系，兄弟关系等等，下面我们依次来看看每一种关系。

**父关系型**
parentNode：每个节点都有一个parentNode属性，它表示元素的父节点。Element的父节点可能是Element，Document或DocumentFragment。
parentElement：返回元素的父元素节点，与parentNode的区别在于，其父节点必须是一个Element，如果不是，则返回null

##### 兄弟关系型api
**previousSibling**：节点的前一个节点，如果该节点是第一个节点，则为null。注意有可能拿到的节点是文本节点或注释节点，与预期的不符，要进行处理一下。
**previousElementSibling**：返回前一个元素节点，前一个节点必须是Element，注意IE9以下浏览器不支持。

**nextSibling**：节点的后一个节点，如果该节点是最后一个节点，则为null。注意有可能拿到的节点是文本节点，与预期的不符，要进行处理一下。
**nextElementSibling**：返回后一个元素节点，后一个节点必须是Element，注意IE9以下浏览器不支持。

##### 子关系型api
**childNodes**：返回一个即时的NodeList，表示元素的子节点列表，子节点可能会包含文本节点，注释节点等。
**children**：一个即时的HTMLCollection，子节点都是Element，IE9以下浏览器不支持。
**firstNode**：第一个子节点
**lastNode**：最后一个子节点
**hasChildNodes方法**：可以用来判断是否包含子节点。

##### 元素属性型api
**setAttribute**
setAttribute：根据名称和值修改元素的特性，用法如下：`element.setAttribute(name, value);`
其中name是特性名，value是特性值。如果元素不包含该特性，则会创建该特性并赋值。
如果元素本身包含指定的特性名为属性，则可以世界访问属性进行赋值，比如下面两条代码是等价的：
```
1 element.setAttribute("id","test");
2
3 element.id = "test";
```
**getAttribute**
getAttribute返回指定的特性名相应的特性值，如果不存在，则返回null或空字符串。用法如下：
`var value = element.getAttribute("id");`

##### 元素样式型api
**window.getComputedStyle**
window.getComputedStyle是用来获取应用到元素后的样式，假设某个元素并未设置高度而是通过其内容将其高度撑开，这时候要获取它的高度就要用到getComputedStyle，用法如下：
`var style = window.getComputedStyle(element[, pseudoElt]);`
element是要获取的元素，pseudoElt指定一个伪元素进行匹配。
返回的style是一个CSSStyleDeclaration对象。
通过style可以访问到元素计算后的样式

**getBoundingClientRect**
getBoundingClientRect用来返回元素的大小以及相对于浏览器可视窗口的位置，用法如下：
`var clientRect = element.getBoundingClientRect();`
clientRect是一个DOMRect对象，包含left，top，right，bottom，它是相对于可视窗口的距离，滚动位置发生改变时，它们的值是会发生变化的。除了IE9以下浏览器，还包含元素的height和width等数据.

本文参考：
http://javascript.ruanyifeng.com ;
https://www.cnblogs.com/lrzw32/p/5008913.html#_label1














