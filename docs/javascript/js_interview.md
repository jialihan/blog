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

Q4.​ ​CSS/JS basic: How do you hide an element in web page? 
- `display:none` - means that the tag in question will not appear on the page at all (although you can still interact with it through the dom). There will be no space allocated for it between the other tags.
- `visibility:hidden` - means that unlike display:none, the tag is not visible, but space is allocated for it on the page. The tag is rendered, it just isn't seen on the page. use `visibility: visible` to enable.
- `opacity: 0` - spaces is allocated for the element, just not visible
- `hidden attribute` - https://www.w3schools.com/tags/att_hidden.asp
	```html
	<p hidden> hide me </p>
	```

Q5.How many ways can you compare two objects in javascript?
// Using JavaScript
JSON.stringify(k1) === JSON.stringify(k2); // true
// Using Lodash
_.isEqual(k1, k2); // true
// write deep equal recursive function
```js
var deepEqual = function (x, y) {
  if (x === y) {
    return true;
  }
  else if ((typeof x == "object" && x != null) && (typeof y == "object" && y != null)) {
    if (Object.keys(x).length != Object.keys(y).length)
      return false;

    for (var prop in x) {
      if (y.hasOwnProperty(prop))
      {  
        if (! deepEqual(x[prop], y[prop]))
          return false;
      }
      else
        return false;
    }

    return true;
  }
  else 
    return false;
}
```

Q6.
第一道题是将link插入到包含每一个用户信息的div里。link的代码是:
<a href="profile.jsp?id=<memeber.id>"><member.name></a>
考点是DOM的操作，怎么向DOM Tree里添加新的节点。
var members = [
	{
	name: 'Bill Denbrough', id: 1
	}, 
	{
	id: 2 
	},
	{
	name: 'Mike Hanlon', id: 3
	}
];
<div id="content">
<a data-id=1 href="profile.jsp?id=[id]">[Member Name]</a> <a href="profile.jsp?id=[id]">[Member Name]</a>
<a href="profile.jsp?id=[id]">[Member Name]</a>
<a href="profile.jsp?id=[id]">[Member Name]</a>
</div>
//insert links for each of the members into the content div <a href="profile.jsp?id=[id]">[Member Name]</a>

// Optimize1: Instead of using <a> tags for performing navigation to the profile page. We want to capture click event and navigate to profile page via javascript.
// window.location = `profile.jsp?id=${el.id}`;
// location.href = `profile.jsp?id=${el.id}`;

// Optimize2: 扩展问题​是如果有很多用户的链接需要一个一个添加到DOM里，会造成reflow影响页面性能，如何解决。答案当然是使用Dom Fragment.
```js
var fragment = document.createDocumentFragment();
members.forEach(el => {
    var itemEL = document.createElement('div');
    itemEL.classList.add('item');
    var itemText = document.createTextNode(el.name);
    itemEL.appendChild(itemText);
    itemEL.onclick = function () {
        // window.location = `profile.jsp?id=${el.id}`;
        location.href = `profile.jsp?id=${el.id}`;
    }
    itemEL.dataset.id = el.id;
    fragment.appendChild(itemEL); // optimize for N times
})
contentEL.appendChild(fragment); // only one time 
```

Q7. Leetcode 273，要考虑小数
// https://leetcode.com/problems/integer-to-english-words/

Q8. 
f(1)(2)(3); // 9
f(2)(2)(1); // 4
f(2,2,1); //4
f(); // 0

```js
const f = (a,b,c)=>{
	if(a && b && c)
	{
		return (a+b)*c;
	}
	if(a & b)
	{
		return (d)=>(a+b)*d;
	}
	if(a)
	{
		return (d)=>(e)=>(a+d)*e;
	}
	return 0; // default value
}
// test
console.log(f(1)(2)(3));
console.log(f(2)(2)(1));
console.log(f(1,2)(3));
console.log(f(2,2,1));
```

Q9. implement a Promise object
https://github.com/jialihan/JavaScript-Onboarding/blob/master/promise-native-implementation/script.js

Q10.第二题，给一个url: http://www.linkedin.com?q1=v1&q2=v2 写一个函数提取query string，返回
{q1: 'v1', q2: 'v2'}
有点坑，需要考虑没query string，或者有fragement的情况 http://www.linkedin.com?q1=v1&q2=v2#xxx 这样子的话就要写regular expression进行处理

<!-- var regex = /[?&]([^=#]+)=([^&#]*)/g,
        url = "www.domain.com/?v=123&p=hello",
        params = {},
        match;
    while(match = regex.exec(url)) {
        params[match[1]] = match[2];
    }

console.log(params); -->


Q11. linkedin 前端过经 + 时间线 + 总结

TIMELINE

08/22 Refer
08/29 Recruiter 联系
09/05 Phone Screen, 当天下午通知 onsite
09/12 Onsite
09/17 送HC
09/20 告知通过面试，进入Team match
09/25 Team matching 结束开始谈offer
09/28 Offer

PHONE SCREEN

问题1： palindrome，简单，秒
问题2： 给一个link，鼠标移到link上会出现一个提示语，是那种bubble chat的样子的，全部HTML/JS/CSS，剩一堆时间问问题

ONSITE

10:00 AM – 11:00 AM | CA for UI | 国人
以下内容需要积分高于 50 您已经可以浏览

给一个数组，求最大的subarray的sum及其坐标
如 [0, -1, 3, 4, -5, 0] 返回 { result: 7, indexes: [2, 3] }
循序渐进，各种解法都过一遍，讨论复杂度，然后最优解写代码(Javascript)，bug free
提前做完，问问题（英文面试，最后中文问问题）


11:00 AM – 11:45 AM | SDA for UI | 两个美国人
以下内容需要积分高于 100 您已经可以浏览

系统设计题，很open end，设计hangman这个游戏，从前端到后端，
疯狂follow up，随机应变
问道最后面试官没问题问我了，然后我问他们问题


11:45 AM – 12:45 PM | LUNCH | ABC？
疯狂扯犊子，午饭也是要记note的

12:45 PM – 01:30 PM | Javascript for UI | 三姐
以下内容需要积分高于 100 您已经可以浏览

设计一个有auto complete功能的dropdown，类似google搜索栏，从前端到后端
楼主主动加上了debounce，infinite scroll 还有 cache的功能，感觉面试官反馈很好
然后要求写代码，讨论特殊情况
提前做完，开始问问题


01:30 PM – 02:15 PM | Product and Technical Taste Hybrid | 亚洲人，不知道哪的
以下内容需要积分高于 100 您已经可以浏览

各种Software Development 的 fundamental，methodology，
然后会问一些工作中的东西，遇到某些情况如何处理，
主要靠经验回答，问题很多随机应变
留了点时间问问题


02:15 PM – 03:15 PM | Apps Host Manager | 黑人大叔
以下内容需要积分高于 100 您已经可以浏览

大多为behavior，还有很多很senior的问题，
比如如何从development到deployment，
某些情况如何做technical decision，
还问了我一些高深的backend的问题，我听都没听说过，就自信满满地说老子不懂（毕竟我是Frontend）
问题都不是很好答，但是千万不要卡壳，也不要one word answer，疯狂聊和圆话就好
留了五分钟问问题


03:15 PM – 04:15 PM | Pragmatic UI | 不知道哪的亚洲人 + 白人姐姐
以下内容需要积分高于 100 您已经可以浏览

纯JS如何在div里头添加一行一行的link，link含人名和这个人profile的链接，
设计一个迷你版的LInkedIn Profile，HTML/JS/CSS  全部都要写
外加一个like功能，以及comment功能
提前做完，开始问问题


SUMMARY

关于面试题：
个人感觉L家的前端的纯算法要求不高，大多是混合问题，但是对实际操作能力要求很高，问题都非常贴近真实工作。
系统设计的follow up和high level的behavior都不好答，一定要随机应变，不要卡在那里

关于答题方式：
疯狂问问题，弄清楚再答，一定要和面试官确定思路没问题再开始写码，然后主动带入Test case，保证代码Bug free
要时常与面试官互动，保证面试官明白你说的东西，没事逗逗他们效果挺好

关于整个面试流程：
感觉L家的Recruiter效率不高，楼主手里有别家Offer疯狂催他们还是磨磨唧唧

心得：
最后一定要很会聊，很会聊，很会聊，加午饭一共七轮，嘴就没有停过的。
然后面试中一个面的好的征兆就是面试官没问题问你了，留很多时间让你问问题，所以当时面完就觉得有offer了

楼主背景：
ME毕业转做Software，一年工作经验

准备面试：
刚毕业那会儿刷了很多题（一年前，400道），
这次一道LeetCode没刷，把Cracking the coding interview详细过了一遍，

个人感觉有一定的刷题基础就好（还是要刷的，楼主不是说不用刷题），主要是思维，还有见招拆招的能力，
我之前刷题的时候会边写边想，导致我在白板和遇到生题的时候容易懵逼和卡壳，
所以我就改成先想好再写码，把任何题都当做生题，遇到各种情况都不慌，

其实面试中不存在真正的超级难题，只要心态好，注重沟通没有做不出来的
毕竟面试官出题就是让你做出来而不是要干死你




写一个类似于点赞功能的ui，点赞之后旁边的数字+1，取消之后-1 写html的时候问了怎么改进accessiblity和symentic?用了一些<header><footer>之类的标签，加了 role属性。
写了个js函数来处理button toggle，事件绑定在外层容器上用了事件代理来处理点击操作


1. 设计一个dashboard，dashboard是一个地图，当有新用户注册账号时，需要实时在dashboard上显
示用户位置?
follow up:前端用什么library?(d3.js)，需要向前端发送哪些数据?(经纬度或者geohash)，push
vs pull?怎么实现push service?(消息队列)?怎么测试?


电面：LC 刘领悟，刘奇艺
Onsite:
https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=728645&highlight=linkedin%2B%C3%E6%BE%AD
第一轮：host manager, 问了比较多我目前组里的dev cycle是什么样的；让我说一下工作中我最proud的一个accomplishment，期间会问些他们感兴趣的问题；问假如很多customer抱怨Linkedin payment用不了了，我如何解决这个问题，分别会先后采取哪些措施
第二轮：technical communication，假如他们对我们做的东西一无所知，让我随便挑一个project介绍，期间会问一些具体技术细节。这轮只要对自己做过的东西比较熟悉就没啥问题了
第三轮：coding1，LC 柳依依(611) 变体，以及 伞肆意(341)
第四轮：coding2，LC 易思久(149)，另外一个真的想不起来了。。但也是LC linkedin 6-month tag下的题
第五轮：系统设计，assuming you have a library to store lots of books, design a system to support two queries: "and" to return the collection of book ids which contain word1 and word2; "not" to return the book ids which don't contain a specific word


是All O`one Data Structure，力扣432，非常巧，正好是力扣凌鹰频率排序第一页hard的题。具体就不说了，题答得完全没问题，有问有答，最后写代码落了几个edge case，和面试官回顾时候被提示了一下，然后马上补上了几个链表操作

一共有7轮，花了一整天

1. Hiring manager，尬聊，一些常见的问题。比如谈一次失败的经历；如果你和其他人意见不一致时怎么处理等。
2. App taste。谈谈你喜欢和不喜欢的App，以及LinkedIn App。我说我不怎么用LinkedIn App，谈谈网页版的行不行，他说没问题。
3. 吃饭，还是纯聊天。这是最简单的一轮，主要是给我们面试者来提问题的。

下面这几轮都是写白板的，连续来好几轮有点伤脑细胞。。。

4. LeetCode上Maximum subarray的增强版，需要同时输出起始和终止的index
5. LeetCode上Number to Word几乎原题。写完后在根据截图写一个支票的HTML布局，以及大致的positioning。我比较习惯flexbox布局就用flexbox写了
6. 写一个memorize function。就是可以根据arguments来缓存return值，第二次输入同样的args不会再计算整个function，而是直接从缓存获取return value。基本就是考Closure和hash的用法。我先写了个single argument function，然后拓展成多个复杂args的function。
7. system design，填字母猜单词的游戏。先讲了讲前端UI布局，前端数据流设计，然后再说需要哪些API，参数和返回值分别是什么，数据库里的数据格式怎样，如何反作弊等。讲完后面试官问我如果想给游戏增加不同难度选项怎么弄，我也大致说了下。

总体给我的感觉是比预想中的简单，面的题目数据结构没有超过JS的范畴。有的公司上来就让我写DFS，你想问你这是招前端么。。。


2019/07
linkedin 前端面筋

Po 主面 linkedin 前端，工作两年。有 offer，没去。

LinkedIn 前端面试极其恐怖。一天 7 轮面试。2 个面试 1 小时，剩下的都是 45 分钟。
中间有 45 分钟时间吃饭。

他们每个面试测试的都是不一样的地方，面试的顺序可能不一样。面试可以用白板 / laptop。所有的 code 必须用 JS 写。

第一轮是系统设计。
这个是一个小时。主要考验你的是宏观的设计。例如后端该有什么 endpoint，前端什么时候要用这些 endpoint。前端每个 page 该怎么和对方交流。一般会叫你设计一个 App。面试我的时候是给设计 linkedin 的 search。这轮之需要画一些 diagram 和 flowchart 就好，不需要写 code。

第二轮是 manager + behavior
这轮就基本是聊以前干过的事情，对未来的规划。然后就非常 general 的 behavior question。例如如果有 deadline 不能 meet 怎么办之类的。

第三轮是 web domain knowledge
这一轮主要问的是 web request 的不同。REST 是什么，endpoint 该怎样设计。query param 什么时候用。web socket 是什么，HTTP 1.1，2.0 有啥不同。还有一些 CSS，HTML 的东西。例如 accessibility 如何实现。啥时候该用 local storage，啥时候该用 cookie。

第四轮是 algorithm
这个就是比较正常的 algorithm 了。不过需要用 JS 来写。algorithm 的难度不大，大概在 LC medium 难度左右。问的是 BFS / DFS 相关的问题

第五轮是 Project
这个也是一个小时。这轮特别难，考的是如何用 JS 写一整个 project。面的是如何写一个 auto complete 的 text box。他会假设你有一个 api 可以返回所有的 suggestion。刚刚开始是写 auto complete 的 input box，就和谷歌的 searchbox 一样。之后会加需求，需要和 gmail 下面的那种 auto complete 的 text field 一样。之后还会加需求，例如要 highlight typo，highlight 语法错误，provide suggestion，点击右键修复语法错误等等。
到最后的需要和 google doc 差不多。

第六轮是 JS
这一轮专门考 JS。需要用 JS 写一些 native 的 lib。面的是写一个改过的 throttle （用 settimeout 或者 set interval 来写）。大概是 throttle 最后一个 request 也要发。

第七轮是 product taste
这一轮比较奇葩。要求你找一个你最喜欢的网站，然后讲你为什么喜欢。然后如果你是PM，你会怎么改进那个网站。考的更多的是你对 human computer interaction 的理解。大多数是学校里学 UI 时候学的东西。