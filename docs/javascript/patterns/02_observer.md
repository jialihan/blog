## JavaScript - Observer Design Pattern

#### I. [What is Observer & Observable ?](#chapter1)

#### II. [Syntax](#chapter2)

#### III. [Native Implementation of these two Classes](#chapter3)

- [3.1 Implementation: Observable Class](#ch3-1)
- [3.2 Implementation: Observer Class](#ch3-2)

#### IV. [RX.Subject Class](#chapter4)

#### V. [More about "Observable" Questions](#chapter5)

<div id="chapter1" />

### I. What is Observer & Observable ?

| Observable                                                                                                                            | Observer                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| - array of obervers: `[]` <br> - register an observer: `subscribe()` <br> - trigger: `fire()` <br> - return an `unsubscribe()` method | - `next(value)` <br> - `error(err)` <br> - `complete()`                                                                            |
| Observables are unicast                                                                                                               | each subscribed Observer owns an independent execution of the Observable, eg: the `observer.next(val)` gets the value and executes |

<div id="chapter2" />

### II. Syntax

create an observable:

```js
const observable = new Observable((subscriber) => {
  subscriber.next(1);
  subscriber.next(2);
  setTimeout(() => {
    subscriber.next(3);
    subscriber.next(4);
    subscriber.complete();
  }, 100);
});
```

create an observer:

```js
const observer = {
  next: (value) => {
    console.log("we got a value", value);
  },
  error: (error) => {
    console.log("we got an error", error);
  },
  complete: () => {
    console.log("ok, no more values");
  }
};
```

Usage:

```js
const sub = observable.subscribe(subscriber);
setTimeout(() => {
  // ok we only subscribe for 100ms
  sub.unsubscribe();
}, 100);
```

<div id="chapter3" />

### III. Native Implementation of these two Classes

<div id="ch3-1" />

#### 1.1 Implementation: Observable Class

**Link**: [BFE #57. create an observable](//%20https://bigfrontend.dev/problem/create-an-Observable)

```js
class Observable {
  constructor(setup) {
    this.setup = setup;
  }
  subscribe(subscriber) {
    var sub = new Observer(subscriber);
    this.setup(sub);
    return {
      unsubscribe() {
        sub.unsubscribe();
      }
    };
  }
}
```

<div id="ch3-2" />

#### 3.2. Implementation: Observer Class

**Property:**

- `.unsubscribe`: default is _false_

**Methods:**

- `next(val)`
- `error(err)`
- `complete()`
- **internal** use: `unsubscribe()`

```js
class Observer {
  constructor(subscriber) {
    if (typeof subscriber === "function") {
      this.subscriber = { next: subscriber };
    } else {
      this.subscriber = subscriber;
    }
    this.unsubscribed = false;
  }
  next(val) {
    if (this.unsubscribed) {
      return;
    }
    if (this.subscriber.next) {
      try {
        this.subscriber.next(val);
      } catch (err) {
        this.error(err);
      }
    }
  }
  complete() {
    if (this.unsubscribed) {
      return;
    }
    if (this.subscriber.complete) {
      this.subscriber.complete();
    }
    this.unsubscribe();
  }
  error(err) {
    if (this.unsubscribed) {
      return;
    }
    if (this.subscriber.error) {
      this.subscriber.error(err);
    }
    this.unsubscribe();
  }
  unsubscribe() {
    this.unsubscribed = true;
    this.subscriber = null;
  }
}
```

<div id="chapter4" />

### IV. RX.Subject Class

**Doc**: http://reactivex.io/rxjs/manual/overview.html#subject

**What is a Subject?**
An `RxJS Subject` is a special type of Observable that allows values to be **multicasted** to many Observers.

- **Every Subject is an Observable.** Given a Subject, you can `subscribe` to it, providing an Observer, which will start receiving values normally.
- Internally to the Subject, `subscribe` does **NOT invoke a new execution** that delivers values. It simply registers the given Observer in a list of Observers, similarly to how `addListener` usually works in other libraries and languages.
- **Every Subject is an Observer.** It is an object with the methods `next(v)`, `error(e)`, and `complete()`.
- To **feed a new value** to the Subject, just call `next(theValue)`, and it will be **multicasted** to the Observers registered to listen to the Subject.

[BFE #71. implement Observable Subject](https://bigfrontend.dev/problem/implement-Observable-Subject)

```js
class Subject {
  constructor() {
    this.observers = [];
    this.next = this.next.bind(this);
    this.complete = this.complete.bind(this);
    this.error = this.error.bind(this);
  }
  next(val) {
    // trigger the multicast
    this.observers.forEach((ob) => ob.next(val));
  }
  error(err) {
    this.observers.forEach((ob) => ob.error(err));
  }
  complete() {
    this.observers.forEach((ob) => ob.complete());
  }
  subscribe(subscriber) {
    // register an observer object
    this.observers.push({
      next: typeof subscriber === "function" ? subscriber : subscriber.next,
      complete: subscriber.complete ? subscriber : () => {},
      error: subscriber.err ? subscriber.error : () => {}
    });
    var lastIndex = this.observers.length;
    return {
      unsubscribe: () => {
        this.observers.splice(lastIndex, 1);
      }
    };
  }
}
```

<div id="chapter5" />

### V. More about "Observable" Questions

- [#70. implement Observable.from()](https://bigfrontend.dev/problem/implement-Observable-from) : [solution - 70](https://github.com/jialihan/js-coding-questions/blob/main/bfe/Observable/bfe_70_observableFrom.js)
- [#72. implement Observable interval()](https://bigfrontend.dev/problem/implement-Observable-interval) : [solution - 72](https://github.com/jialihan/js-coding-questions/blob/main/bfe/Observable/bfe_72_inteval.js)
- [#73. implement Observable fromEvent()](https://bigfrontend.dev/problem/implement-Observable-fromEvent) : [solution - 73](https://github.com/jialihan/js-coding-questions/blob/main/bfe/Observable/bfe_73_fromEvent.js)
- [#74. implement Observable Transformation Operators](https://bigfrontend.dev/problem/implement-Observable-transformation-operators) : [solution - 74](https://github.com/jialihan/js-coding-questions/blob/main/bfe/Observable/bfe_74.js)
