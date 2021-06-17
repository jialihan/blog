### Date Object in Javascript

#### create a new date

```
var date = new Date();
```

#### Month Result

##### Month is `0-based`

```
date.getMonth(); // (January gives 0)
```

#### Different to get current date

Reference: [stackoveflow-link](https://stackoverflow.com/questions/33184096/date-new-dated-valueof-vs-date-now/33184171)

- `new Date()` - creates a Date object representing current date/time
- `new Date().valueOf()` - returns number of milliseconds since midnight 01 January, 1970 UTC
- `new Date().getTime()` - Functionally equivalent to new Date().valueOf()
- `Date.now()` - returns the number of milliseconds since midnight 01 January, 1970 UTC
