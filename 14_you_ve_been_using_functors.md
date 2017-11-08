# You've been using Functors

*Functor*: Any type with `map` method.

*Functor Laws*: 

1. *Composition*: `fx.map(f).map(g) == fx.map(x => g(f(x)))`
    
```javascript
const res1 = Box('squirrels')
  .map( s => s.substr(5) )
  .map( s => s.toUpperCase() )

const res2 = Box('squirrels')
  .map( s => s.substr(5).toUpperCase() )

//res1 == res2
```

2. *Identity*: `fx.map(id) == id(fx)`

```javascript
const id = x => x
const res1 = Box('crayons').map(id)
const res2 = id(Box('crayons'))

//res1 == res2
```