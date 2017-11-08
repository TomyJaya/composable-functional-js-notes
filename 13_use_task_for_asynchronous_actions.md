# Use Task for Asynchronous Actions

In the below code, readFile and writeFile is async. We need to use `Task` for them. 

However, their asynchronous behavior is supported by using the traditional JavaScript callback. 

Fortunately, there already exists an existing library, such as [futurize](https://github.com/futurize/futurize) which can convert callback-based functions automatically to `Task`-based functions. 

Nevertheless, below is how we can do it the manual way: 

```javascript
// Before
const app = () => 
    fs.readFile('config.json', 'utf-8', (err, contents) => {
        if (err) throw err
        const newContents = contents.replace(/8/g, '6'))
        fs.writeFile('config1.json', newContents, (err, _) => {
            if (err) throw err
            console.log('success!')
        })
    })

// After
const readFile = (fileName, encoding) =>
  new Task((rej, res) =>
    fs.readFile(fileName, encoding, (err, contents) =>
      err ? rej(err) : res(contents)
  ))

const writeFile = (fileName, contents) =>
  new Task((rej, res) =>
    fs.writeFile(fileName, contents, (err, success) =>
      err ? rej(err) : res(success)
  ))

const app =
  // read file - returns Task
  readFile('config.json', 'utf-8')
  // modify contents - plain function inside
  .map( contents => contents.replace(/8/g, '6') )
  // write modified content into new file
  // function with lifted target inside
  .chain( contents => writeFile('config1.json', contents) )

// only now launch the task
app.fork( showErr, _ => showSuc("Check 'config1.json'!") )
```

Using futurize: 

```javascript
const { futurize } = require('futurize')
const future = futurize(Task)

// wrap Node's native read and write files into Futures
// (i.e. tasks to be run in the future)
const readFuture = future(fs.readFile)
const writeFuture = future(fs.writeFile)

// setup the task
const app1 = readFuture('config.json', 'utf-8')
    .map( contents => contents.replace(/8/g, '6') )
    .chain( contents => writeFuture('config2.json', contents) )

// run the task
app1.fork( showErr, _ => showSuc("Check 'config2.json'!") )
```