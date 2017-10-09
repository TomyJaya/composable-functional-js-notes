# Ensure failsafe combination using monoids

*Monoid* is *Semigroup* with a special element which acts as a neutral identity. E.g. 

1. In `Sum` Semigroup, `3 + 0 = 3`. `0` is the identity
2. In `Array` Semigroup, `[1,2,3] + [] = [1,2,3]`. `[]` is the identity.

```javascript

// For Sum 
Sum.empty = () => Sum(0)
const res = Sum.empty().concat(Sum(1).concat(Sum(2))) // Sum(3)

// For All
All.empty = () => All(true)
const res2 = All(true).concat(All(true).concat(All.empty())) // All(true) 
const res3 = All(false).concat(All(true).concat(All.empty())) // All(false) 

```

`First` can't be a Monoid because it doesn't have a natural identity element. 

*Conclusion*: Monoid is safer than Semigroup as it can take neutral element (e.g. None) and still proceed. 