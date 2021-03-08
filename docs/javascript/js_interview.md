## JS interview  question
Q1. what is the difference create an instance using: new ?
	- `var x = new Person();
	- `var x2 = Person();` ~~// wrong, undefined~~

	The [new](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) keyword doing:
		1)  Creates a blank, plain JavaScript object.
		2)  Adds a property to the new object (`__proto__`) that links to the constructor function's prototype object
		    
		    Properties/objects added to the construction function prototype are therefore accessible to all instances created from the constructor function (using  `new`).
		    
		3)  Binds the newly created object instance as the  `this`  context (i.e. all references to  `this`  in the constructor function now refer to the object created in the first step).
		4)  Returns  `this`  if the function doesn't return an object.

Q2. Tell me about event bubbling. How could you use it?
   What is bubble down? What is bubble event?
   给个具体的例子?
   知道capture phase吗? 2，什么是 event delegation?优缺点?
- API, when an event occurs in an element inside another element, and **both elements have registered a handle** for that event.   
- When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.​
- With `bubbling`, the event is first captured and handled by the innermost element and then propagated to outer elements.
- With `capturing`, the event is first captured by the outermost element and propagated to the inner elements.
- Edge case: a `​focus​ event` does **not bubble.**
- Event Phase: [docs-MDN](https://developer.mozilla.org/en-US/docs/Web/API/Event/eventPhase)
	```js
	e.eventPhase == 0 ? "none" :
	e.eventPhase == 1 ? "capturing" :
	e.eventPhase == 2 ? "target" :
	e.eventPhase == 3 ? "bubbling" : "error";
	```
- difference between  `event.target` & [`event.currentTarget`](https://developer.mozilla.org/en-US/docs/Web/API/Event/currentTarget) (=== "this")
	-   event.target​ – is the “target” element that initiated the event, it doesn’t change through the bubbling process.
	-  this​ – is the “current” element, the one that has a currently running handler on it.

Q3. 给了一张图片，要求用HTML/CSS实现其中的布局 = 给个tooltip图片，照着写html, css. 一个图片 ，让你用html写出来. 大概是一个linkedin的用户推荐界面。

第一行是文字People You May Know 我用div和p，​提示让用<h5>tag

下面三小行:每一行是一个推荐给你的用户. 又分三小列:每一列从左到右是:一个图片，两行文字，一个 叉叉图标。其中第一行文字是:姓名(加粗的)，职位，第二行文字是:一个加号，一个“connect”，要求 用<a></a>加在connect旁边。最后右下角有个See more>>> 我还没写，他就问了两个CSS问题: ​前面的代码都 不用写JavaScript和CSS，只用html，但他要问你为什么用这个tag):

1. 有边框如何实现，边框变圆如何实现.(border-radius) 2. H5的字体大小如何实现(use css selector: h5{ font-size:24px;})

Version2: ​至于widget, 就是然你写一个tooltip。没有限定说用什么，不过题目是直接在一个HTML页面上， 个人感觉最好用是jQuery或vanilla JS，因为25分钟内，要set up别的UI framework的话，会浪费很多时间。全 称可以google一些方法，因为他们也是知道没人会在实际工作中用vanilla JS 做DOM manipulation. 在写得过 程当中， 面试官会问一些相关的问题。比如，你怎么写CSS，让tooltip固定在相关element的特​定位​置。 event target vs current target等等。

Version3: ​加实现tooltip(Both html css 方法 and js方法​) 如何当hover在一个div上时显示tooltip。需要自己 写html和JS, use `.tooltip:hover .tooltip-text {display: block;}`

Q4.






