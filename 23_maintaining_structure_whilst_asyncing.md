# Maintaining structure whilst asyncing

Let's say we need to do some aync operation which returns result: 

```javascript
const fs = require('fs')
const Task = require('data.task')
const { List, Map } = require('immutable-ext')

const httpGet = (path, params) =>
   Task.of(`${path} result`)
```

on an existing structured object: 

```javascript
// Need to wrap vanilla object in immutable-ext's Map for traverse
const routes = Map({
  home: '/',
  about: '/about-us',
  blog: '/blog'
})
```

we can just do the below: 

```javascript
routes
  .traverse(Task.of,route => httpGet(route, {})) 
  // Task(Map({home: '/ result',about:'/ about-us', ...})
```

Notice the result will have Task on the outside as we use `traverse` instead of `map`.


Even nested structure can be preserved:

```javascript
// Map of route arrays
Map({
  home: ['/', '/home'],
  about: ['/about-us']
})
.traverse(
  Task.of,

  // now routes are arrays,
  // wrap them into List and traverse
  // to return Task
  routes =>

    // need to wrap into 'List' providing 'traverse'
    List(routes).traverse(
      Task.of, route => httpGet(route, {})
    )
)
.fork( console.error, console.log )
```