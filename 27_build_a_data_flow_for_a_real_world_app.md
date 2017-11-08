# Build a data flow for a real world app

The final working code can be found here: [https://github.com/DrBoolean/spotify-fp-example](https://github.com/DrBoolean/spotify-fp-example)

*Spotify API*: [https://developer.spotify.com/web-api/](https://developer.spotify.com/web-api/)

## Goal
Given 2 artirts, we want to suggest another artist by getting the common (intersection of) related artists of those 2. 

## Dependencies to Install
`npm install data.either data.task fantasy-identities request --save`

## Approach

Rather than building `class`es in the typical OO way, we'll build data flow and run them through functions. 

## Main

```javascript

const Task = require('data.task')

// wrap arguments in Task to remain pure
const argv = new Task((rej,res) => res(process.argv))
const names = argv.map(args => args.slice(2)) // since the first 2 tokens are 1) node 2) index.js


const related = name => 
  findArtist(name)
  map(artist=> artist.id)
  .chain(relatedArtist)

const main = ([name1,name2]) =>
  // since we're working with 2 independent data, we should use applicatives
  Task.of(rels1 => rels2 => [rels1,rels2])
  .ap(related(name1))
  .ap(related(name2))

names.chain(main).fork(console.error, console.log)
```

