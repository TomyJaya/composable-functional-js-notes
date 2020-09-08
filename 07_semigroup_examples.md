# Semigroup Examples

In the following use case, we want to *combine* two accounts.

```javascript
const acct1 = {name: 'Nico', isPaid: true, points: 10, friends:['Franklin']};
const acct2 = {name: 'Nico', isPaid: false, points: 2, friends:['Gatsby']};

```

The word *combine* should alert us on the use-case of *Semigroup*s. 

```javascript
// Need the below library as JS's bare object doesn't have concat
const { Map } = require('immutable-ext');

//1. wrap all the properties in Semigroups
//2. wrap the objects in immutable-ext's Map to have a concat method
const acct1 = Map({name: First('Nico'), isPaid: All(true), points: Sum(10), friends:['Franklin']});
const acct2 = Map({name: First('Nico'), isPaid: All(false), points: Sum(2), friends:['Gatsby']});

const res = acct1.concat(acct2).toJS();
// Don't forget to call toJS() to unwrap it
```

Notice that object's properties should also be `Semigroup`s for the `concat` on the parent to work. 
Also, we won't need to wrap immutable-ext's `Map` in functional languages like Scala or Haskell. 
