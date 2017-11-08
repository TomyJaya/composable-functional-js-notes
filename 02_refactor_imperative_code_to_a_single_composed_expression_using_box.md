# Refactor imperative code to a single composed expression using Box

### Existing Code to Refactor
```javascript
const moneyToFloat = (str) => 
    parseFloat(str.replace(/\$/g, ''))


const percentToFloat = str => {
    const replaced = str.replace(/\%/g, '');
    const number = parseFloat(replaced);
    return number * 0.01;
}

const applyDiscount = (price, discount) => {
    const cost = moneyToFloat(price);
    const savings = percentToFloat(discount);
    return cost - cost * savings;
}

const result = applyDiscount('$5.00','20%')
console.log(result);
```

### Replace with `Box` (from previous lesson)
Use box to unnest expressions:

```javascript
const moneyToFloat = str => 
    Box(str)
    .map(s => s.replace(/\$/g, ''))
    .map(r => parseFloat(r))


const percentToFloat = str => {
    .Box(str => str.replace(/\%/g, ''))
    .map(replaced => parseFloat(replaced);
    .map(number => number * 0.01);
}

const applyDiscount = (price, discount) => {
    moneyToFloat(price)
    .fold(cost => 
       percentToFloat(discount)
       .fold(savings => 
          cost - cost * savings);
}

const result = applyDiscount('$5.00','20%')
console.log(result);
```

### Notes:

1. Notice how we can have variables in the inner function via closure.
2. If we didn't use fold in the last `applyDiscount` function, we'll end up with `Box` within a `Box` (two layers deep). We'll learn how to remove this awkwardness in the next sessions. 