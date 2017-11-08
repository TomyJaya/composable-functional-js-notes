# Principled type conversions with Natural Transformations

*Natural Transformation*: Structurally converting a functor to another functor (e.g. `Either` to `Task`)

Example: 
```javascript

const eitherToTask = e =>
  e.fold(Task.rejected, Task.of)


```

If we call with `Right`:

```javascript
eitherToTask(Right('nightingale'))
    .fork(
    e => console.error('Error: ', e),
    res => console.log('Result: ', res)
    ) // Result: nightingale 
```

Using `Left`: 

```javascript
eitherToTask(Right('nightingale'))
    .fork(
    e => console.error('Error: ', e),
    res => console.log('Result: ', res)
    ) // Error: nightingale 

```

## Natural Tranformation Law

Any function which satisfies the below law is a *Natural Transformation*:
`nt(x).map(f) == nt(x.map(f)`

Example. 
```javascript

boxToEither = b => 
  b.fold(Right)
```

Why `Right`? Not `Left`? Had we used Left, the natural transformation law will be violated: 

```javascript

boxToEither = b => 
  b.fold(Left)

//Notice the different location of the parens
const res1 = boxToEither(Box(100)).map(x => x * 2); // nt(x).map(f)
const res2 = boxToEither(Box(100).map(x => x * 2)); // nt(x.map(f)
```

`res1` will be `Box(200)` whilst `res2` will be `Box(100)`. 

The below diagram visually describes the law: 

![Image of Natural Transformation](https://i.stack.imgur.com/FdSWk.jpg)