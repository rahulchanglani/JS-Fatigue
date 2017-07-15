Generators
==========

When the first time I saw it in Redux Saga, I was a bit surprised at the way it works. To wrap my head around the generator fundamentals, I checked out MDN and other blogs. In the meantime, I also write code to play with it. After one hour’s try, I think I finally get it and want to bullet point the main features with illustrative code.

Let me list them first and then go through each of them.

> 1.First invocation of next does not accept arguments
> 2.Return yielded value
> 3.Yielded value replacement
> 4.Iterate through iterator returned by generator
> 5.Short circuit generator
> 6.Yield Precedence


### First invocation of next does not accept arguments

The first call of next will not take any arguments, even though you pass them in, they will be thrown away.

```Javascript
function* test() {
 let a = yield 'a';
 a = yield a + 'b';
 yield a + 'c';
}
var t = test();
t.next('replace').value; // a
```

Another thing to keep in mind is that the number of next you call to reach to the completion is always one more than yield count. What I mean is if you see two yield in your generator, you need to call next at least 3 times to get to completion.

```Javascript
function* test() {
 let a = yield 'a';
 a = yield a + 'b';
 yield a + 'c';
}
var t = test();
t.next('replace').done; // false
t.next().done; // false
t.next().done; // false
t.next().done; // true
```

### Return yielded value

Every time, generator execute the yield, it will pause on that point and return the value right after it.

```JavaScript
function* test() {
 let a = yield 'a';
 a = yield a + 'b';
 yield a + 'c';
}
var t = test();
t.next().value; // a
t.next().value; // undefined b
t.next().value; // undefined c
```

Why I get output like undefined bSee next point.


### Yielded value replacement

next method can also take argument which will then be used as the last returned value.

```Javascript
function* test() {
 let a = yield 'a';
 a = yield a + ' b';
 yield a + ' c';
}
var t = test();
t.next('xxx').value; // argument dropped
t.next('yyy').value; // yyy b
t.next().value; // undefined c
```

If next is triggered with no argument, it will act as undefined is passed in. That is why you get undefined c in the last call.

[generator_01](https://cdn-images-1.medium.com/max/800/1*iHA2OYIL-SJ21dKL8PRtRg.png)

Let’s modify it a bit to have different returned values.

```Javascript
function* test() {
 let a = yield 'a';
 b = yield a + ' b';
 yield a + ' c';
}
var t = test();
t.next('xxx').value; // argument dropped
t.next('yyy').value; // yyy b
t.next().value; // yyy c
```

Recall the screen dump above? Now b is replaced with undefined which will leave a untouched, that is why we get yyy cas the result.

If you do not assign the yield value, then passing arguments will not affect the returned value.

```Javascript
function* test() {
 yield 'a';
 yield 'b';
 yield 'c';
}
var t = test();
t.next('*').value; // a
t.next('**').value; // b
t.next('***').value; // c
Another tricky example:
function* foo() {
 var arr = [yield 1, yield 2, yield 3];
 console.log(arr, yield 4); // [null, 3, null], 'now complete'
}
var f = foo();
f.next(); // 1
f.next(null); // 2
f.next(3); // 3
f.next(null); // 4
f.next('now complete');
```

See screen dump below for annotations.

[generator_02](https://cdn-images-1.medium.com/max/800/1*2qkWHH-NgnFrXt1o9FMJOw.png)

Code snippet below shows that only the yielded value is replaced, not the whole statement evaluated value.

```JavaScript
function* test() {
 let a = yield 'a';
 let b = yield a + ' b';
 let c = ' c' + (yield a + ' d');
 yield c + ' e';
}
var t = test();
t.next().value;
t.next().value;
t.next().value;
t.next('xxx').value; // cxxx e
```

a + ' d' is replaced by xxx rather than value of variable c.


## Iterate through iterator returned by generator

Call generator will basically return an iterator:

```JavaScript
function* test() {
 yield 'a';
 yield 'b';
 yield 'c';
}
var t = test();
for (var v of t) {
 console.log(v); // a b c
}
```

## Short circuit generator

Call return on next will simply complete the generator. Any given arguments will then be returned to the caller.

```JavaScript
function* test() {
 yield 'a';
 yield 'b';
 yield 'c';
}
var t = test();
t.next(); // a
t.return('good') // good
t.next('bad'); // { value: undefined, done: true }
```

## Yield Precedence

Only … spread operator and , comma operator have lower precedence than yield, all other operators will be evaluated first.

```JavaScript
function* test() {
 let a = yield 'a';
 let b = yield a + ' b';
 let c = ' c' + (yield a + ' d');
 yield a + ' e';
}
var t = test();
t.next().value;
t.next().value; // undefined b
t.next('xxx').value; // undefined d
```

## Summary

That’s pretty much it. It appears generator by itself does not produce much value though. However, with help of co, you can write amazing async operations that behave like sync ones. I am still looking to see what I can do with it without using any libraries. If anybody has some ideas, please leave the comments below. Thanks.
