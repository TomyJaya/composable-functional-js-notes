# Create types with Semigroups

Semigroup is a type with `concat` method. e.g. `String`  & `Array`

```javascript
const res = 'a'.concat('b').concat('c')
const res2 = [1,2].concat([3,4]).concat([5,6]);
```

From abstract algebra, Semigroup has the *Associativity* property. I.e.:

`(a + b) + c == a + (b + c)`

We can make our custom semigroups:

```javascript
const Sum = x => 
({
    x,
    concat: ({x:y}) => 
        Sum(x+y),
    inspect: () =>
        `Sum(${x})`
})

const res3 = Sum(1).concat(Sum(2)).concat(Sum(3)); // Sum(6)

// All Semigroup's expected behaviour:
// 1. true && false => false
// 2. true && true => true

const All = x => 
({
    x,
    concat: ({x:y}) => 
        All(x && y),
    inspect: () => 
        `All(${x})`
})

const res4 = All(true).concat(All(true)).concat(All(false)); // All(false)

const First = x => 
({
    x,
    concat: _ => 
        First(x),
    inspect: () => 
        `First(${x})`
})

const res4 = First('blah').concat(First('asd')); // First('blah')
```
