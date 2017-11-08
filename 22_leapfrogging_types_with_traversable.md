# Leapfrogging types with Traversable

# Problem Statement

Sometimes, we end up with the below: 

`Array[Task[R]]`

where `R` is your result, but what you actually need is: 

`Task[Array[R]]`.


Whenever we want to leapfrog/ commute these 2 types, we can use `traverse`


Example:
```javascript
const fs = require('fs')
const Task = require('data.task')
const { List } = require('immutable-ext')
const futurize = require('futurize').futurize(Task)

// wrap node API to return Task
const readFileTask = futurize(fs.readFile)

 // wrap array to List as it doesn't have traverse
const files = List(['config.json', 'config1.json'])

const oriRes = files.map(fn => readFile(fn,'utf-8')) // Array[Task[R]]

// Instead, we can use traverse:

const oriRes = files
  .traverse(Task.of,fn => readFile(fn,'utf-8')) // Task[Array[R]]
  .fork(console.error, console.log)
```

### Notes:

1. we needed to use `Task.of` as a type help as JavaScript is not a statically typed language
2. Not all types are traversable
3. the function you pass into the `traverse` needs to return an applicative functor as it relies on `ap` to work under the hood