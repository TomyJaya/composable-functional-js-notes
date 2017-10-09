# Delay Evaluation with `LazyBox`

Unlike `Box`, instead of taking a value, `LazyBox` takes in a function which returns the value. 

Its purpose is to maintain purity (no side-effect) by virtue of laziness.  

It delays executing the function when `map`ped, and only gets invoked during `fold`

```javascript
const Box = x => ({
    map: f => Box(f(x)),
    fold: f => f(x),
    inspect: _ => `Box(${x})`,
})

const LazyBox = g => ({
  fold: f => f(g())
  map: f => LazyBox( () => f(g()) ),
})

const res = LazyBox( () => '   64 ' )
    .map(s => s.trim())
    .map(r => parseInt(r))
    .map(i => i + 1)
    .map(i => String.fromCharCode(i))
    .fold(s => s.toLowerCase())

console.log(res)

```

This is how `Promise`s and `Observable`s define the `map` implementation.