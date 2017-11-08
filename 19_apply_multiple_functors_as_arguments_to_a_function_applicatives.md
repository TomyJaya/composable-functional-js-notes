# Apply multiple functors as arguments to a function (Applicatives)

The below example will run one at a time (one after the other): 

```javascript
const Either = require('../either')

const liftA2 = (f,fx,fy) => 
    fx.map(f).ap(fy)

const $ = selector => 
  Either.of({selector, height: 10})

const getScreenSize = (screen, head, foot) => 
  screen - (head.height + foot.height)

$('header').chain(head => 
  $('footer').map(footer => 
    getScreenSize(800,head,footer)
  )
)
```

Applicatives will help to run them together:

```javascript
//First, we need to curry the function
const getScreenSize = screen => head => foot => 
  screen - (head.height + foot.height)


const res = Either.of(getScreenSize(800)) // partially apply 
            .ap($('header'))
            .ap($('footer'))

// Using liftA2
const res2 = liftA2(getScreenSize(800),$('header'),$('footer'))        
```