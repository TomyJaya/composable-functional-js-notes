# Apply Natural Transformations in everyday work


## When to use Natural Tranformation? 

1. JavaScript `Array` (`[]`) doesn't have `chain`. We can use `List` from immutable-ext. 

```javascript
const { List } = require('immutable-ext')

List(['hello','world'])
  .chain(x => List(x.split('')))

```

2. For performance optimization, we can use natural transformation law to mechanically rewrite the below: 

```javascript
const first = xs =>
  fromNullable(xs[0])


const largeNumbers = xs =>
  xs.filter( x => x > 100 )

const larger = x =>
  x * 2

// The below will unnecessarily double the items in all the array, then taking the first
const app = xs =>
  first(largeNumbers(xs).map(larger))

console.log(app([2, 400, 5, 1000]))
```

From `nt(x).map(f) == nt(x.map(f)`, we know that we can rewrite the below app as follows: 

```javascript

const app = xs =>
  first(largeNumbers(xs))
  .map(larger))

```

3. To work with multiple Container types. For example, the code below tries to find the user's best friend. 

```javascript
// fake user returned by id for testing purposes
const fake = id => ({
  id: id,
  name: 'user1',
  best_friend_id: id + 1
})

const Db = ({
  find: id => new Task(
    (rej, res) =>
      // simulate error 'not found' for id <= 2
      res( id > 2 ? Right(fake(id)) : Left(fake('not found')) )
  )
})

// natural transform Either to Task
const eitherToTask = e =>
  e.fold(Task.rejected, Task.of)


Db.find(3) // Task(Right(user))
  .map(either => 
   either.map(user => Db.find(user.best_friend_id))) // Oh my God, what's the type here? 
   // Right(Task(Right(User)) ??? 

```

Ideally, we want to be able to `chain`, but since we're working with 2 different nested container types, we can make use of natural transformation. 

```javascript
Db.find(3)
.chain(eitherToTask)
.chain(user => Db.find(user.best_friend_id))
.chain(eitherToTask) // Need to transform again as Db.find returns an Either again
.fork(console.error, console.log)
```