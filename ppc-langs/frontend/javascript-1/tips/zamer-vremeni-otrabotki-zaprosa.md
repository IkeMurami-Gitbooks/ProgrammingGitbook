# Замер времени отработки запроса

```javascript
var t0 = performance.now()

doSomething()   // <---- measured code goes between t0 and t1

var t1 = performance.now()
console.log("Call to doSomething took " + (t1 - t0) + " milliseconds.")
```

или в лог (строки в time и timeEnd должны быть одинаковы):

```javascript
console.time('doSomething')

doSomething()   // <---- The function you're measuring time for 

console.timeEnd('doSomething')
```
