# rxjs workshop with Ben Lesch
## pre-course videos
### arrow functions
```javascript
function foo(x) {
  return x + x;
}
var foo2 = (x) => x + x;
```
- return what is right after the arrow
- if there is only 1 argument, parenthesis are options
```javascript
var obj = {
  x: 'hi!',
  foo: () => {
    return this.x + '!'; // this undefined because this is from scope outside obj
  }
  foo2: function() {
    return this.x = '!'; // works because not an arrow function, inherits this from parent
  }
}
```
### Observable.of()
- simplest way to create an observable
- from tc39 spec, have in rxjs
```javascript
const source = Rx.Observable.of('hi', 'there', 'you');

console.log('start')
source.subscribe(x => console.log(x))
console.log('end')
// start
// hi
// there
// you
// end
// => synchronous by default
const source = Rx.Observable.of('hi', 'there', 'you', Rx.Scheduler.async);
// change it to next async tick, start/end will happen before subscription
```
### Observable.from()
- convert other types of objects into observables
- handle promises
- handle any iterable
  - careful with strings as it will iterate through the characters in it
```javascript
console.log('start')
Observable.from([1,2,3])
  .subscribe(x => console.log(x));
console.log('stop')
// start
// 1
// 2
// 3
// stop
```
- with generator functions
```javascript
const gen = function * () {
  yield 1;
  yield 2;
  yield 3;
  return;
}

console.log('start')
Observable.from(gen())
  .subscribe(x => console.log(x))
console.log('stop')
// start
// 1
// 2
// 3
// stop
```
- with observableesque [Symbol.observable]
```javascript
const obj = {
  [Symbol.observable]() {
    return {
      subscribe(observer) {
        observer.next(1);
        observer.complete();
        return {
          unsubscribe() {
            console.log('tear down');
          }
        }
      }
    }
  }
}
```
### Observable.interval()
```javascript
Observable.interval(500)
  .subscribe(x => console.log(x)) // waits 500ms and then starts
Observable.timer(500)
  .subscribe(x => console.log(x)) // fires once after 500ms
Observable.timer(0, 1000)
  .subscribe(x => console.log(x)) // fires immediately and then once every second
```
### Observable.map
```javascript
const arr = [1,2,3];
console.log(arr.map(x => x + '!'));
// creates a brand new array, doesn't mutate arr
// loops over entire array

const source$ = Observable.of(1,2,3)
const result$ = source$
  .map(x => {
    console.log('map 1', x)
    return x + '!'
  })
  .map(x => {
    console.log('map 2', x)
    return x + '?'
  })
// map 1
// 1
// map 2
// 1!
// 1!?
// => passes each element through the chain and emits before moving onto next element
```
### Observable.filter
```javascript
const arr = [1,2,3];
const result = arr.filter(x => x % 2 === 0)
console.log(result)
// [2,4]

const source$ = Observable.of(1,2,3,4)
const result$ = source$
  .map(x => {
    console.log('map 1: ' + x)
    return x + 1
  })
  .filter(x => x % 3 === 0)
  .map(x => {
    console.log('map2: ' + x)
    return x + 1
  })

result$.subscribe(x => console.log(x))
// map 1: 1
// map 1: 2
// map 2: 3
// 4
// map 1: 3
// map 1: 4
```
### Observable.scan
- use a reducer to keep state but emit values over time
- can use to create a redux store where value is an action
```javascript
const source$ = Observable.of(1,2,3,4)
source$.scan((state, value) => state + value, 0)
  .subscribe(x => console.log(x))
// 1
// 3
// 6
// 10
```

## course videos
### subscribe with callback
```javascript
data$.subscribe(
  x => console.log(x),
  err => console.error(err),
  () => console.log('done')
)
```

### subscribe with observer
```javascript
const observer = {
  next(x) { console.log(x) },
  error(err) { console.log(err) },
  complete() { console.info('done') }
}
data$.subscribe(observer)
```

### unsubscribe
```javascript
const sub = data$.subscribe(
  x => console.log(x),
  err => console.error(err),
  () => console.info('done')
)
setTimeout(() => {
  sub.unsubscribe()
}, 1000)
// stream never completes
```

### subscribe with forEach
```javascript
data$.forEach(x => console.log(x))
  .then(
    () => console.log('done'),
    err => console.error(err)
  ) // returns a promise once the observable completes

// NOTE: If `forEach` returns a promise, how an we unsubscribe?
//   We can't (yet! perhaps in the future of Rx?)
```

### observable constructor
```javascript
const source$ = new Rx.Observable(observer => {
  observer.next(1)
  observer.next(2)
  observer.next(3)
  observer.complete();
}) // this is a synchronous observable
source$.subscribe(
  x => console.log(x),
  err => console.error(err),
  () => console.info('done')
)
```

### observable ctor resource
```javascript
/**
Custom resource interop
NOTE: `Resource` usage:
const resource = new Resource(); // start the resource;
resource.addEventListener('data', handler); // listen for data events
resource.removeEventListener('data', handler); // stop listening for data events
*/
const source$ = new Rx.Observable(observer => {
  const resource = new Resource()
  const handler = x => observer.next(x)
  
  resource.addEventListener('data', handler)

  return () => { // teardown logic
    resource.removeEventListener('data', handler)
  }
})
const subscription = source$.subscribe(
  x => console.log(x),
  err => console.error(err),
  () => console.info('done')
)
setTimeout(() => subscription.unsubscribe(), 2100);
// Resource: started
// Resource: event listener added
// 0
// 1
// 2
// 3
// Resource: event listener removed
// Resource: closed
```

### observable of
```javascript
```

### observable from array
```javascript
```

### observable from promise
```javascript
```

### observable from iterable
```javascript
```

### observable from event
```javascript
```

### array map filter reduce
```javascript
```

### observable map filter reduce
```javascript
```

### subjects
```javascript
```

### subject subscriptions
```javascript
```

### subject observer
```javascript
```

### subject multicasting
```javascript
```

### behavior subject
```javascript
```

### replay subject
```javascript
```

### composite subscription
```javascript
```

### declarative subscription
```javascript
```
