# Build curried functions

*Currying*: Technique of preloading functions with argument

> Currying is converting a single function of n arguments into n functions with a single argument each.

```javascript
const add = x => (y => x + y)
const inc = add(1) // y => 1 + y
const res = inc(2) // 3


const replace = regex => repl => str =>
  str.replace(regex, repl)

// still accepts data 'str' as argument
const censor = replace(/[aeiou]/g)('*')
console.log(censor('hello world'))


const map = f => xs => xs.map(f)

// partially apply our 'censor' to array
const censorAll = map(censor)
console.log(censorAll(['hello', 'world']))

```

