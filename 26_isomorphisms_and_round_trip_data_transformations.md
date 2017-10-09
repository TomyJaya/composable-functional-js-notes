# Isomorphisms and round trip data transformations

*Isomorphism*: 2 functions `from` and `to` which satisfies `from(to(x)) == x && to(from(y)) == y`. 

E.g. `String ~ [Char]` String and chacter array. 

```javascript
const Iso = (to, from) => 
({
    to, 
    from
})

const chars = Iso(s=> s.split(''), c => c.join(''))

const res = chars.from(chars.to('hello world'))
console.log(res) // hello world

const resChars = chars.to('hello world')
console.log(resChars) //[ "h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d" ]

```

## Use of Isomorphism

We can use functionality in one type to perform some operations and convert it back to the original type. 

For example, we can implement `truncate` using array's functionalities (e.g. `slice`) . 

```javascript
const truncate = str => 
  chars.from(chars.to(str).slice(0,3)).concat('...')
  
```

## Another example of Isomorphism

```javascript
// [a] ~ Either null a

const singleton = Iso(

  // Either into Array
  // 'fold' is used to extract value
  e => e.fold( () => [], x => [x] ),

  // Array into Either
  ([x]) => x ? Right(x) : Left()
)

// filtering Either type objects
// transform to Array, filter, than back to Either
const filterEither = (e, pred) =>
  singleton.from(
    singleton.to(e)
    .filter(pred)
  )

const res = filterEither(
  Right('hello'),
  x => x.match(/h/ig)
)
.map( x => x.toUpperCase() )

const res1 = filterEither(
  Right('ello'),
  x => x.match(/h/ig)
)
.map( x => x.toUpperCase() )

console.log(res, res1)
```