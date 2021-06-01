## Reflow/Repaint and Minimize DOM manipulation

#### 1. [What DOM operations will cause reflow?](#p1)  

#### 2. [How to minimize DOM manipulation ?](#p2)  
- [Merge multiple DOM manipulations into one
](#p2-1)
- [Hide/unmount elements before DOM manipulation](#p2-2)
- [Change position to fixed/absolute on animated elements](#p2-3)
- [Reduce DOM query/get manipulation](#p2-4)
- [addEventListener only on Parent node](#p2-5)


<div id="p1" />

### 1. What DOM operations will cause reflow?

- 添加或者删除可见的DOM元素；  
- 元素位置改变；  
- 元素尺寸改变——边距、填充、边框、宽度和高度  
- 内容改变——比如文本改变或者图片大小改变而引起的计算值宽度和高度改变；  
- 页面渲染初始化；  
- 浏览器窗口尺寸改变——resize事件发生时；

Modern browser's optimization: 
现代浏览器会针对重排或重绘做性能优化，比如，把DOM操作积累一批后统一做一次重排或重绘。

But: 但在有些情况下，浏览器会立即重排或重绘。
比如请求如下的DOM元素布局信息：
- `offsetTop/Left/Width/Height`
- `scrollTop/Left/Width/Height`
- `clientTop/Left/Width/Height`
- `getComputedStyle()` OR  `currentStyle`

因为这些值都是动态计算的，所以浏览器需要尽快完成页面的绘制，然后计算返回值，从而打乱了重排或重绘的优化。

<div id="p2" />

### 2. How to minimize DOM manipulation?

Docs: [article](https://www.huaweicloud.com/articles/a4805a6a133f1b8f7f08546e359a4d5d.html)

<div id="p2-1" />

#### 2.1 Merge multiple DOM manipulations into one

**Example 1: minimize CSS rules**
frequently modify DOM element's styles, similar code below:
```js
element.style.borderColor = '#f00';  
element.style.borderStyle = 'solid';  
element.style.borderWidth = '1px';`
```

**Optimized Code:**
```js
// solution 1  
element.style.cssText += 'border: 1px solid #f00;';  
// solution 2 : .empty defined in css file
element.className += 'empty';
```
Compare two solutions:
- solution 1: **better performance** 
- solution 2: cost more performance due to the query of the `className`. But **better code maintenance.**

**Example 2: minimize html changes**

类似的操作还有通过innerHTML接口修改DOM元素的内容。不要直接通过此接口来拼接HTML代码，而是以字符串方式拼接好代码后，一次性赋值给DOM元素的`innerHTML`接口。

<div id="p2-2" />

#### 2.2 把DOM元素离线或隐藏后修改

这种方式适合那些需要大批量修改DOM元素的情况。具体的方式主要有三种：
- use DOM Fragment
	Article: [docs](https://www.cnblogs.com/echolun/p/10098752.html), [DocumentFragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment), [createDocumentFragment()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createDocumentFragment)
	Template code:
	```js
	var fragment = document.createDocumentFragment();  
	var fragment2 = new DocumentFragment(); // same with prev line
	// your dom manipulation on this fragment 
	// ...		
	document.getElementById('container').appendChild(fragment);
	```
- 通过设置DOM元素的display样式为none来隐藏元素
	这种方式是通过隐藏页面的DOM元素，达到在页面中移除元素的效果，经过大量的DOM操作后恢复元素原来的display样式。对于这类会引起页面重绘或重排的操作，就只有隐藏和显示DOM元素这两个步骤了。	
	Template code:
	```js
	var myElement = document.getElementById('myElement');  
	myElement.style.display = 'none';  
	// your dom manipulation on myElement 
	// ...  
	myElement.style.display = 'block';
	```
	
- clone DOM element to memory
 这样一来，影响性能的操作就只是最后替换元素的这一步操作了，在内存中操作克隆元素不会引起页面上的性能损耗。
Template Code:
	```js
	var old = document.getElementById('myElement');  
	var clone = old.cloneNode(true);  
	// your dom manipulation on clone element
	// ...  
	old.parentNode.replaceChild(clone, old);
	```

<div id="p2-3" />

#### 2.3 设置具有动画效果的DOM元素的position属性为fixed或absolute

把页面中具有动画效果的元素设置为绝对定位，使得元素脱离页面布局流，从而避免了页面频繁的重排，只涉及动画元素自身的重排了。这种做法可以提高动 画效果的展示性能。

<div id="p2-4" />

#### 2.4 Reduce DOM query/get manipulation

获取DOM的布局信息会有性能的损耗，所以如果存在重复调用，最佳的做法是尽量把这些值缓存在局部变量中。
For example:  `element.offsetTop` is computed repeatedly

```js
for (var i=0; i < len; i++) {  
	myElements[i].style.top = targetElement.offsetTop + i*5  +  'px';  
}
```

**Fix:** store the values from dom into a **variable** in memory.
```js
var targetTop = targetElement.offsetTop;  
for (var i=0; i < len; i++) {  
myElements[i].style.top = targetTop+ i*5  +  'px';  
}
```

<div id="p2-5" />

#### 2.5 addEventListener only on Parent node
**Problem:** 
在DOM元素上绑定事件会影响页面的性能，一方面，绑定事件本身会占用处理时间，另一方面，浏览器保存事件绑定，所以绑定事件也会占用内存。页面中 元素绑定的事件越多，占用的处理时间和内存就越大，性能也就相对越差，所以在页面中绑定的事件越少越好。

**Fix:**
- 即利用事件冒 泡机制，只在父元素上绑定事件处理，用于处理所有子元素的事件，
- 在事件处理函数中根据传入的参数判断事件源元素，针对不同的源元素做不同的处理。

**Template Code:**
```js
// get parent node then add a click event  
document.getElementById('container').addEventListener("click",function(e) { 
	// check each event.target
	if(e.target && e.target.nodeName.toUpperCase == "LI") { 
		// TODO
	}  
});
```
