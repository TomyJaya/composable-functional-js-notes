# Applicative Functors for multiple arguments

## Problem Statement

When we have a function and we want to apply this function to multiple Functor arguments, we can put the function in the Functor and then call `ap`. 

`ap` is like the inverse of `map`. e.g. 

```javascript
const Box = x => (
    {
    ap: b2 => b2.map(x)
    // other existing Box methods...
})
const res = Box(x => x + 1).ap(Box(2)) // Box(3)
```

You can keep applying arguments.

```javascript
const res = Box(x => y => x + y).ap(Box(2)).ap(Box(3)) // Box(5)
```

*NOTE*: the function you pass into the Box should be in curried form!

## Formal Definition of Applicative Functors

> *Applicative Functors* are Functors with `ap` method


`F(x).map(f) === F(f).ap(F(x))`

where `F` is a Functor, `x` is a value, and `f` is a function.  

## Helper functions for Applicative Functors

`liftA2`: takes vanilla function, and 2 containerized arguments 

```javascript
const liftA2 = (f,fx,fy) => 
    fx.map(f).ap(fy)

// example: 
const add = x => y => x + y
const res = liftA2(add, Box(2), Box(3)) // Box(5)
```

`liftA3` ... `liftAN`: takes vanilla function, and N containerized arguments

```javascript
const liftA3 = (f,fx,fy, fz) => 
    fx.map(f).ap(fy).ap(fz)

// ...

```

