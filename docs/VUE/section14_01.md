## Section 14_01 Infinite scrolling in Vue

### 1. open questions

- when to perform a request for more songs?
- to ensure not making multiple request

### 2. decide scrolling position

2.1 event listener for "scroll"

- add event listener when page inital loads
- [remove event listener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener#matching_event_listeners_for_removal) when component `unmounts`.

```js
window.addEventListener("scroll", this.handleScroll);
window.removeEventListener("scroll", this.handleScroll);
```

2.2 calc height positions
复习：

- [infite scroll doc](https://jialihan.github.io/blog/#/javascript/infinite-scroll?id=p2)
- ✅ `srollHeight = scrollTop + clientHeight`
- ❌ `OffsetHeight = window.innerHeight + scrollTop`
