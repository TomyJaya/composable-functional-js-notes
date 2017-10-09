# Unbox types with foldMap

Calling `fold` of a `List` 'summarizes' the list (extract one value).

```javascript
import { Map, List } from 'immutable-ext'
const res = List.of(Sum(1),Sum(2),Sum(3))
            .fold(Sum.empty()) // Sum(6)
```  

It's the same intutition as how fold on a Box extracts the value *one level out*. 
E.g. 

* `Box(T)` -> `fold` -> `T`
* `List(Sum(T))` -> `fold` -> `Sum(T)`


If we have the below: 

```javascript
const res = Map.of({brian:Sum(3),sara:Sum(2)})
            .fold(Sum.empty()) // Sum(5)
```

What if we have `Map.of({brian:3,sara:2})` instead? It's cool. We can `map` to wrap it in `Sum`. 

```javascript
const res = Map.of({brian:3,sara:2})
            .map(Sum)
            .fold(Sum.empty()) // Sum(5)


```

Same case with the below:

```javascript
const res = List.of(1,2,3)
            .map(Sum)
            .fold(Sum.empty()) // Sum(6)
```

The combination of `map`ping and then `fold`ing is so common that `foldMap` is introduced. 

```javascript
const res = List.of(1,2,3)
            .foldMap(Sum, Sum.empty()) // Sum(6)
```