# Find the intersection of sets with Semigroups

## Revised main 

To find the intersection of an array, we can define `Intersection` semigroup. 

```javascript
const Intersection = xs =>
({
  xs,
  concat: ({xs: ys}) =>
    // You can actually optimize the low-level implementation here
    // as it's hidden 
    // e.g. use for-loop
    Intersection(xs.filter(x => ys.some(y => x === y)))
})

const artistIntersection = rels1 => rels2 => 
  Intersection(rels1).concat(Intersection(rels2)).xs

```


What if we want to change our program to accept more than 2 artists? 

```javascript
const artistIntersection = rels =>
  rels
  .foldMap(x => Pair(Intersection(x), Sum(x.length)))
  .bimap(x => x.xs, y => y.x)
  .toList()

const main = names =>
  List(names)
  .traverse(Task.of, related) // Flip List of Tasks to Task of List
  .map(artistIntersection)
```


What if we want to know how many of such comparison occurs? 

```javascript
const { Pair, Sum } = require('./monoid')

const artistIntersection = rels =>
  rels
  .foldMap(x => Pair(Intersection(x), Sum(x.length)))
  .bimap(x => x.xs, y => y.x) // bimap runs the function on each of the functor
  .toList()
```
