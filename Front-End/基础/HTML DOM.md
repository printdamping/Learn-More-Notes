# HTML DOM

## 1. DOM 简介

DOM 是 Document Object Model（文档对象模型）的缩写。

DOM 是 W3C（万维网联盟）的标准。

DOM 定义了访问 HTML 和 XML 文档的标准：

> “W3C 文档对象模型 （DOM） 是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。”

HTML DOM 是：

- HTML 的标准对象模型
- HTML 的标准变成接口
- W3C 标准

HTML DOM 定义了所有HTML元素的**对象和属性**，以及**访问它们的方法**。也就是说，HTML DOM 是关于如何获取、修改、添加或删除 HTML 元素的标准。

## 2. DOM 节点

在 HTML DOM 中，所有事物都是节点。DOM 是被视为节点树的 HTML。

 根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：

- 整个文档是一个文档节点
- 每个 HTML 元素是元素节点
- HTML 元素内的文本是文本节点
- 每个 HTML 属性是属性节点
- 注释是注释节点

HTML DOM 将 HTML 文档视作树结构，这种结构被称为**节点树**：

![HTML DOM Node Tree](HTML DOM.assets/ct_htmltree.gif)

通过 HTML DOM，树中的所有节点均可通过 JavaScript 进行访问。所有 HTML 元素（节点）均可被修改，也可以创建或删除节点。

## 3. DOM 方法

> **核心**

浏览器网页就是一个 DOM 树形结构！

- 更新：更新 DOM 节点
- 遍历 DOM 节点：得到 DOM 节点
- 删除：删除一个 DOM 节点
- 添加：添加一个新的节点

要操作一个 DOM 节点，就必须先获得这个 DOM 节点。

- `getElementById(id)` ：返回带有指定 ID 属性的元素
- `getElementByTagName(tag)` ：返回指定标签的元素，标签选择器，比如 p 、h1
- `getElementByClassName(class)` ：返回指定 class 属性的元素

```html
<div id="father">
    <h1>标题一</h1>
    <p id="p1">p1</p>
    <p class="p2">p2</p>
</div>

<script>
    'use strict';
    var div = document.getElementById('father');
    var h1 = document.getElementsByTagName('h1');
    var p1 = document.getElementById('p1');
    var p2 = document.getElementsByClassName('p2');        
</script>
```

这是原生代码的实现，之后用 jQuery 来完成。

> 更新节点

```js
// 修改节点文本
div.innerText = '123';
// 修改HTML内容
div.innerHTML = '<strong>123</strong>';
// 操作JS
div.style.color = 'red';
div.style.fontSize = '32px';
```

> 删除节点

删除节点步骤

1. 获取父节点
2. 调用父节点删除子节点

```js
// 删除子元素self
var self = document.getElementById('p1');
var father = self.parentElement;
father.removeChild(self);

// 移除指定子节点、
father.removeChild(father.children[0]);
father.removeChild(father.children[0]);
father.removeChild(father.children[0]);
```

删除多个元素的时候，注意删除是一个动态的过程，所以实际上每次都删除第一个就可以。

> 插入节点

获得了某个 DOM 节点，假设这个节点是空的，可以通过 `innerHTML()` 直接插入一个元素，但是这个 DOM 节点已经存在元素时，就无法这样做了，否则会覆盖原元素。

**给已存在的节点添加新子节点：**

```html
<p id="js">javascript</p>
<div id="list">
    <p id="se">javaSE</p>
    <p id="ee">javaEE</p>
    <p id="me">javaME</p>
</div>
<script>
    'use strict';
    var 
    	js = document.getElementById('js'),
        list = document.getElementById('list');
    list.appendChild(js);
</script>
```

效果：

<img src="HTML DOM.assets/image-20210730233227290.png" alt="添加新节点" style="zoom:80%;" />

`appendChild()` 给已存在的节点添加子节点。

**创建一个完全的新节点：**

```html
<script>
    'use strict';
    var newP = document.createElement('p');
    newP.id = 'newP';
    newP.innerText = '新创建的节点';
    document.body.appendChild(newP);
</script>
```

**创建一个带属性的标签节点：**

```html
<script>
    'use strict';
    var myScript = document.createElement('script');
    myScript.setAttribute('type', 'text/javascript');
    document.body.appendChild(myScript);
</script>
```

使用 `setAttribute()` 来了设置属性。

