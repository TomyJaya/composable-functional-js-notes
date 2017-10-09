# List comprehensions with Applicative Functors

The below pattern of nested loop:

```javascript
for (a in x) {
    for (b in y) {
        for (c in z) {
            // do domething
        }
    }
}
```

can be replaced by List & Applicative Functors:

```javascript
const merchandise = () => 
    List.of( x => y => z => `${x}-${y}-${z}`)
    .ap(List(['T-shirt', 'sweater']))
    .ap(List(['large', 'medium', 'small']))
    .ap(List(['black', 'white']))

```

Benefit of this *List Comprehension* pattern:
1. Declarative 
2. Can easily add another case without cracking open the loop
3. efficient and easy to optimize
