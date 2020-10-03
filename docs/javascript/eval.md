## Why eval is a bad idea to in JavaScript?

It's not that _`eval()`_ optimizes, it's that it  prevents JavaScript engines to optimize, since it accepts a string and they cannot do static analysis on the code it may execute.

* **Malicious code injection**
invoking `eval` can crash a computer. For example: if you use `eval` server-side and a mischievous user decides to use an [infinite loop](https://stackoverflow.com/questions/24977456/how-do-i-create-an-infinite-loop-in-javascript) as their username

* **Slow performance**
 code with `eval()` executes slower than normal javascript code

* **Difficult to Debugging**