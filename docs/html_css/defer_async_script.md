## Defer vs. Async in script tag

- defer: asynchronously download the JS file, but only execute after dom rendered. **In order** with the code sequence written in html.
- async: asynchronously download the JS file, but execute it immediately after download. Maybe in **disorder**.
- Note: `<head>` JS file will always be downloaded first.

```html
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="./test1.js"></script>
    <link rel="stylesheet" href="https://www.google.com/css/maia.css" />
  </head>
  <body>
    <script defer src="./test3.js"></script>
    <script defer src="./test2.js"></script>
    <script async src="./test4.js"></script>
  </body>
</html>
```

**Output:**

```
test 1
test 4 - async
test 3 - defer
test 2 - defer
```

**Profiler:**
![image](/assets/defer_async_script.png ":size=1000x250")

**Reference doc:**
https://segmentfault.com/q/1010000000640869/a-1020000000641029
https://juejin.cn/post/6844903560879013896
