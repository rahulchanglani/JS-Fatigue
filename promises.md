Promises
========

Promise has been out in the wild for a few years and heaps of API libraries wrap them up for users to make async operations with ease. We all know we can make the async call as below

```javascript
someAsyncCall.then(res => res.data).then();
```

But I am sure some beginners do not know how Promise works and what they need to be mindful when using or building their own promise.
If you are among those, take your time to read through. I will cover everything about Promise inside out.
I will refer successful and failed callbacks set on then to onResolved and onRejected throughout the writing.
Code below will be used for illustration.

```javascript
const delJSON = (url) => {
  return new Promise((resolve, reject) => {
    setTimeout(function() {
      if (url) {
        resolve({
          message: 'deleted',
          id: 2
        });
      } else {
        reject({
          error: 'delJSON failed'
        });
      }
    }, 500);
  });
};
const getJSON = (url) => {
  return new Promise((resolve, reject) => {
    setTimeout(function() {
      if (url) {
        resolve({
          data: [1, 2],
          id: 1
        });
      } else {
        reject({
          error: 'getJSON failed'
        });
      }
    }, 2000);
  });
};
```

## Basics

### Three states of Promise

A Promise can only be one of these three states as follow:
>1. Pending: Initial state
>2. Resolved: Operation succeeded
>3. Rejected: Operation failed


The only way for **Pending** to turn into **Resolved** or **Rejected** is by calling ```resolve(value)``` or ```reject(error)```.

Once the state is changed, you cannot roll back to Pending. It will stay permanently.

### Promise initialisation

Call constructor Promise to return an instance.

```javascript
promise.then(function(data) {
  // success
}, function(error) {
  // failure
});
```

Promise constructor takes a function as its only one argument that has ```resolve``` and ```reject``` as the function parameters. Calling resolve with value will trigger ```onResolved``` that is set by calling then on a Promise instance. Similarly, calling reject with error will trigger ```onRejected```.


_onRejected_ callback is optional but it is highly suggested to set it in order to handle the errors.
One thing to keep in mind is that Promise acts as a container which contains the future result of operations. They are executed once they are created.

```javascript
let promise = new Promise(function(resolve, reject) {
  console.log('start');
  resolve();
});
promise.then(function() {
  console.log('done');
});
console.log('ok');
// start
// ok
// done
```


### Then

All promise instances have then method which is used to add two callbacks : onResolved and onRejected.

```javascript
const onResolved = (data) => {return data.id};
const onRejected = (error) => {};
promise.then(onResolved, onRejected).then(id => console.log(id));
```

You can chain then by returning the value which is then be wrapped into another new promise instance.


### Catch

catch is a sugar syntax for .then(null, onRejected) which is triggered when there is an error.

```javascript
getJSON().then(data => {
}).catch(err => {
  // {error: 'getJSON failed'}
});
```


### All

You can use Promise.all to run multiple operations.

```javascript
const p = Promise.all([p1, p2, p3]);
```

Typically, Promise.all takes an array of promise instances which determines the final state for the returned promise from calling Promise.all.

Let’s look at how we resolve p first.
Basically, for p to be resolved, p1,p2,p3 all need to be resolved.

```javascript
const p = Promise.all([getJSON('baidu.com'), delJSON('google.com')]);
p.then(data => data).catch(err => console.log(err));
// [{data: [1,2], id: 1}, {message: 'deleted', id: 2}]
```

As you can see, the resolved values will be passed to onResolved in the same order as we create all promise instances.

Now, let’s look at the rejected case. If anyone of instances gets rejected, p will be rejected and the reason for the firstly-rejected promise will be passed to onRejected.

```javascript
const p = Promise.all([getJSON('baidu.com'), delJSON()]);
p.then(data => console.log(data)).catch(err => console.log(err));
// {error: 'delJSON failed'}
```

### Race

It is quite self-explanatory — the first resolved or rejected promise will determine the state of the promise instance created by race.

```javascript
const p = Promise.race([getJSON('baidu.com'), delJSON('google.com')]);
p.then(data => console.log(data)).catch(err => console.log(err));
// { message: 'deleted', id: 2 }
```


### Advanced

#### Resolve with another promise instance
I would like to call this scenario a nested promise. Nested promise will determine the outside promise state.

```javascript
getJSON('baidu.com').then(data => {
  console.log(data); // not called
}).catch(err => {
  console.log('the error ', err);
  // {error: 'operation failed'}
});
const delJSON = (url) => {
  return new Promise((resolve, reject) => {
    setTimeout(function() {
      if (url) {
        resolve({
          message: 'deleted',
          id: 2
        });
      } else {
        reject({
          error: 'delJSON failed'
        });
      }
    }, 500);
  });
};
```

In the above example, you might think then will be called, but that is not the case, instead catch will be triggered since nested promise delJSON takes the control. In other words, inner promise state overrides the outside one.


#### Error handling

You might wonder which is the preferred way to handle errors between setting onRejected or using catch. The answer is latter — catch. The problem with onRejected is if an error happens in onResolved, onRejected will simply turn bind eyes to it whereas catch will catch it!

```javascript
getJSON('baidu.com').then(data => {
  [].move();
}, (err) => {
  console.log('operation failed', err);  // turn blind eyes to error
});
delJSON('google.com').then(data => {
  [].move();
}).catch(err => {
  console.log('operation failed', err); // caught! since I see it
});
```

You can also use catch for catching errors of separate operations.

```javascript
getJSON('baidu.com').then(data => {
  data.move();
}).catch(err => {
  console.log('1st error ', err);
}).then(data => {
  data.move();
}).catch(err => {
  console.log('2nd error ', err);
});
// prints both errors
```

_Catch_ allows you to catch errors in these places effectively.
* Promise constructor function argument
* onResolved
* functions returning a new promise instance in onResolved

However, error below will not be caught since promise is already resolve and state stays and will not be affected by any operations.

```javascript
var promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('not ok');
});
promise
  .then(function(value) { console.log(value) }) // ok
  .catch(function(error) { console.log(error) }); // not called
```

#### Promise.resolve and Promise.reject

We can also call these two methods on Promise constructor with primitive values.

```javascript
Promise.resolve('foo');
new Promise(resolve => resolve('foo'));
// above expressions are the same
Promise.reject('bar')
new Promise(reject => reject('bar'));
```

The executions will create a promise instance with state in resolved or rejected.


#### Promise.all runs in series or parallel ?

This puzzles a lot of people and I see this question is being asked by users on stackoverflow. Actually, all promise instances are already run by the time we call Promise.all with them.

```javascript
const p = Promise.all([delJSON('google.com'), getJSON('baidu.com']);
```

Recall I describe promise as a container that keeps the future result of operations earlier? Promise.all just waits until all promise are resolved and then pass the resolved values down to onResolved. So, strictly speaking, we cannot say Promise.all is either run in series or parallel.


That’s it, probably all you need to know Promise. For the sake of quick review, I list key points as follow.

* Three states : pending, resolved, rejected
* Call Promise constructor with a function as the only one argument which takes resolve and reject two callbacks
* For a successful/failed operation, call resolve with data or reject with error which will in turn trigger then or catch
* Nested promise determines outside promise’s states
* Use catch rather than onRejected for better error coverage
* Call Promise.resolve or Promise.reject with primitive values will basically generate a new promise instance
* Promise.all only cares about the state not in what order the promise are resolved
* Use then().catch().then().catch() to handle errors in different operations
