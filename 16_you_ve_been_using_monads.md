# You've been using Monads

*Monad*: 
1. Container with `of` and `chain` (aka `flatMap`, `bind`, `>>=`)
2. Allow nest computations
3. "Monads are pointed functors that can flatten"
4. Monads are also applicative functors

## `chain` and `join`
`chain` is `map` and then `join` (flatten). 


```javascript
httpGet('/user')
  .chain(user => 
    httpGet(`/comments/${user.id}`)
    .chain(comments => 
      updateDom(user,comments))) // Without chain Task(Task(Task(DOM)))

// Box(Box(x)) => join => Box(x)
const join = m => 
  m.chain(x => x)
```

## Monad Laws/ Properties

```javascript
// associativity
const m = Box(Box(Box(3)))
const res1 = join(m.map(join))
const res2 = join(join(m)

// identity
const m1 = Box('wonder')
const res21 = join(Box(m1))
const res22 = join(m1.map(Box))
```
Why we need to know these properties? So that we can mechanically replace one for the other when required. 

## Why called Monad?
Why not uppable joinable? 
The word comes from years of research on Category Theory. It's better for us to stay close to its root. 

## Monads and Their uses

| Monad  | Use                         |
|--------|-----------------------------|
| Either | error handling              |
| Task   | asynchronous & side effects |
| List   | Non-determinism             |

## External Reference
https://github.com/MostlyAdequate/mostly-adequate-guide/blob/master/ch9.md