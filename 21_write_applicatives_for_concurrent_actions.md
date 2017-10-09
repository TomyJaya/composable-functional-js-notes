# Write applicatives for concurrent actions

The below will wait for `Db.find(20)` to finish before calling `Db.find(8)`: 
```javascript

const Task = require('data.task')

const Db = {
  find: id => new Task(
      (rej, res) => setTimeout(
        _ => res( {id: id, title: `Project ${id}`} ),
        100
      )
    )
}

const reportHeader = (p1, p2) =>
  `Report: ${p1.title} compared to ${p2.title}`

Db.find(20).chain(p1 => 
  Db.find(8).map(p2=> 
    reportHeader(p1,p2)))
```

If `p2` doesn't depend on `p1`, instead, if we use applicative, both will be kicked off asynchronously!

```javascript
// remember to curry the function
Task.of( p1 => p2 => reportHeader(p1, p2) )
  .ap(Db.find(20))
  .ap(Db.find(8))
  .fork(console.error,console.log)
```