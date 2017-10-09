# Capture Side Effects in a Task

`Task` has `fork` interface which accepts 2 arguments: 

1. function handling the error 
2. function handling the success value

```javascript
// From Folktale library
const Task = require('data.task')

const launchMissiles = _ =>
  new Task((rej, res) => {
    console.log("launch missile!")
    res("missile")
  })

launchMissiles()
  .map(x => x + "!" )
  // only this will actually perform the task
  .fork(e => console.log('err :', e), 
        x => console.log('success: ', x)) // missile!

```

Notes: 
1. you should wrap side effects in `Task`
2. without calling `fork`, side effect won't be invoked
3. you can defer the `fork`ing to the caller of your function
4. the caller of your function can even still extend and compose on it