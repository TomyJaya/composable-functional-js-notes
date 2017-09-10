# Create linear data flow with container style types (Box)

### Problem with normal imperative Style
When you read the usual imperative style, data and control flow somehow feel 'jumpy'.  
```javascript
const nextCharForNumberString = str => {
    const trimmed = str.trim(); // method call
    const number = parseInt(trimmed); //  function call
    const nextNumber = number + 1; // operator
    return String.fromCharCode(nextNumber); // class function
}

// Can be re-written in a terser yet more confusing way: 
const nextCharForNumberString2 = str => 
    String.fromCharCode(parseInt(str.trim()) + 1)

```

### Container-style programming to the rescue
Container-style programming will help to fix this issue. The idea is to unify all those operations in a linear data flow. We can do this using JavaScript's built-in container, Array. 

```javascript
const nextCharFromNumberString = str => {
    [str]
    .map(s => s.trim())
    .map(r => parseInt(r))
    .map(i => i + 1)
    .map(i => String.fromCharCode(i))
}
```
`map` allows for composition within a context.  

### Introducing our first Container type: Box
Formally, let's declare our first container type, `Box`, which has 3 methods: `map`, `fold`, `inspect`.   

```
const Box = x => 
({
    map: f => Box(f(x)), // wrap it in a Box again for chaining
    fold: f => f(x), // remove it from the Box
    inspect: () => `Box($x)` // to facilitate console.log
})
```

So, finally, we can write: 
```
const nextCharFromNumberString = str => {
    Box(str)
    .map(s => s.trim())
    .map(r => parseInt(r))
    .map(i => i + 1)
    .fold(i => String.fromCharCode(i))
}
```

### Final Notes:
1. Box is actually the Identity functor
2. Some poeple argue that this style of programming theoretically creates a performance issue (especially in JS). Nevertheless, there's actually no proven significant impact people experience. The readability and maintainability gain outweighs the slight performance hit. Unless we're writing embedded systems, we shouldn't worry too much about this performance. 