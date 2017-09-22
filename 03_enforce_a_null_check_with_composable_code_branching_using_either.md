# Enforce a null check with composable code branching using Either

### Introducing `Either`

`Either` is an interface. Its implementation is either `Right` or `Left`.

`Right` defined as follows:

```javascript
const Right = x => 
({
    map: f => Right(f(x)),
    inspect: () => `Right(${x})`,
    fold: (f, g) => g(x) 
})
```

Notice how it's similar to `Box`. The only difference is its `fold` method which accepts 2 functions.  

`Left`, on the other hand, doesn't do anything when a function is passed to it. It's a pass-through container. 

```javascript
const Left = x => 
({
    map: f => Left(x), // notice that f is not invoked
    inspect: () => `Left(${x})`,
    fold: (f, g) => f(x)
})
```

Why does fold need 2 functions? Simple! It's because most of the time, you don't know whether you have a `Left` or a `Right`. Thus, you need 2 functions: one for when it's `Left` and one for when it's `Right`. 

Let's check our setup here:

```javascript
// Using Right
const result = Right(2).map(x => x + 1).map(x => x / 2).fold(x => 'error',x => x);
console.log(result); // 1.5

// Using Left
const result = Left(2).map(x => x + 1).map(x => x / 2).fold(x => 'error',x => x);
console.log(result); // error <- ignore our requests
```

`Either` is a useful pure functional way to capture error, conditional branching, null checks, and many more. It's formally known as Disjunction. 

Let's see `Either` in action here: 

```javascript
const findColor = name => 
  ({red: '#ff444', blue:'#3b5998', yellow: '#fff68f'})[name]

const result = findColor('red').slice(1).toUpperCase();
console.log(result); // 'FF444'
```

But if we supply "green": 

```javascript
const result2 = findColor('green').slice(1).toUpperCase(); // undefined error
```


### `Either` to the rescue: 
```javascript
const safeFindColor = name => {
    const found = ({red: '#ff444', blue:'#3b5998', yellow: '#fff68f'})[name];
    // returns either a Right of Left
    return found ? Right(found) : Left(null);
}
  
// Notice that since safeFindColor returns an Either, 
// You need to map over the result
const result3 = safeFindColor('green')
                .map(c => c.slice(1))
                .fold(e => 'no color',
                      c => c.toUpperCase());

console.log(result3); // 'no color'               

const result4 = safeFindColor('red')
                .map(c => c.slice(1))
                .fold(e => 'no color',
                      c => c.toUpperCase());

console.log(result4); // 'FF444'     
```

For convenience, we can define the below: 

```javascript
const fromNullable = x => 
    x != null ? Right(x) : Left(null)
```

and use it like:

```javascript
const safeFindColor = name => 
    fromNullable(({red: '#ff444', blue:'#3b5998', yellow: '#fff68f'})[name]);
```
