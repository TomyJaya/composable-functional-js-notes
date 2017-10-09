# Lift into a Pointed Functor with of

`.of` is a generic interface to lift a value to a container type (constructor agnostic). 

*Pointed Functor*: Functor with `of` method.

```javascript

Task.of('hello') // Task('hello')
Either.of('hello') // Right('hello')

```
