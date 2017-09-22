# Use `chain` for composable error handling with nested `Either`s


### Problem Definition


The following imperative code: 
```javascript
const fs = require('fs')

const getPort = () =>
    try {
        const str = fs.readFileSync('config.json');
        const config = JSON.parse(str);
        return config.port;
    } catch(e) {
        return 3000;
    }
```

with a helper method:
```javascript
const tryCatch = f => {
    try {
        return Right(f());
    } catch(e) {
        return Left(e);
    }
}
```

can be written in a composable way: 
```javascript
const getPort = fileName =>
  tryCatch( () => fs.readFileSync(fileName) )
    .map(c => tryCatch(() => JSON.parse(c))) // JSON.parse can cause an error, we need to guard it with a tryCatch
    .fold(
      e => 3000,
      c => c.port)
```

But this results in nested `Either`s. 

### `chain` to the rescue

```javascript
const Right = x => 
({
    chain: f => f(x),
    // ...
})

const Left = x => 
({
    chain: f => Left(x), // still same as map
    // ...
})
```

Now, instead of `map`, when we expect a nesting, we'll use `chain` instead.  

```javascript
const getPort = fileName =>
  tryCatch( () => fs.readFileSync(fileName) )
    .chain(c => tryCatch(() => JSON.parse(c))) // chain will help with the un-nesting
    .fold(
      e => 3000,
      c => c.port)
```

### `fold` vs. `chain`

* `fold`: the idea of removing/ extracting the value from container
*  `chain`: expect a function to run and return the same type