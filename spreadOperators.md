Spread Operator ES6
====================

### What does it look like? 

Three dots: ...

### What does it do? 

The spread operator allows an expression to be expanded in places where multiple elements/variables/arguments are expected.


Boring stuff out of the way, let’s look at a bunch of examples to better understand what in the heck the spread operator actually is.

> 1. Create an array called middle
> 2. Create a second array which contains the middle array
> 3. Log out the result

```javascript
var middle = [3, 4];
var arr = [1, 2, middle, 5, 6];
console.log(arr);
// [1, 2, [3, 4], 5, 6]
```

Above, we don’t use the spread operator. By doing this, when the arr array is created, the middle array is inserted into the middle of the arr array as an array within an array.
This is fine if that’s the goal, but what if we want only a single array?
We can use the spread operator. Again, the spread operator allows the elements of our array to expand. Take a look at the code below, everything is the same — except we’re using the spread operator:

```javascript
var middle = [3, 4];
var arr = [1, 2, ...middle, 5, 6];
console.log(arr);
// [1, 2, 3, 4, 5, 6]
```

Remember the spread operator definition you just read above? Here’s where it comes into play. As you can see, when we create the arr array and use the spread operator on the middle array, instead of just being inserted, the middle array expands. Each element in the middle array is inserted into the arr array. This means that instead of nested arrays, we are left with a simple array of numbers ranging from 1 to 6.


### Slice

slice() is a JavaScript Array method that allows us to copy arrays. In a similar fashion, we can use the spread operator to copy an array:

```javascript
var arr = ['a', 'b', 'c'];
var arr2 = [...arr];
console.log(arr2);
// ['a', 'b', 'c']
```

Voila! We have a copied array. The array values in arr expanded to become individual letters which were then assigned to arr2. We can now change the arr2 array as much as we’d like with no consequences on the original arr array.

Again, the reason this works is because the value of arr is expanded to fill the brackets of our arr2 array definition. Thus, we are setting arr2 to equal the values of arr not to arr itself. This idea might make more sense if we look at what happens without the spread operator.

If we simply create an array then set another array equal to it, here’s what it would look like:

```javascript
var arr = ['a', 'b', 'c'];
var arr2 = arr;
console.log(arr2);
// ['a', 'b', 'c']
```

Now, you might be saying, “That worked”. It looks the same as above!
While it may look the same, it is very different under the hood. Instead of creating a copy and assigning arr2 to each individual value of arr, what we’ve actually done is set arr2 equal to then entire arr array. This means that anything we do to arr2 will also effect the original arr array.

```javascript
arr2.push('d');
console.log(arr2);
// ['a', 'b', 'c', 'd']
console.log(arr);
// ['a', 'b', 'c', 'd']
```

As you can see, even though we did not explicitly add the new value to our original arr, our new value d was still pushed to the end of the arr array.


### Concat

Instead of using concat() to concatenate arrays, we can also use the spread operator. First, lets see what this would look like using concat()

```javascript
var arr = ['a', 'b', 'c'];
var arr2 = ['d', 'e', 'f'];

arr1 = arr.concat(arr2);
console.log(arr);
// ['a', 'b', 'c', 'd', 'e', 'f']
Now, lets use the spread operator:
var arr = ['a', 'b', 'c'];
var arr2 = ['d', 'e', 'f'];
arr = [...arr, ...arr2];
console.log(arr);
// ['a', 'b', 'c', 'd', 'e', 'f']
```

The exact same result! Each array expanded to allow the array values to be set in the new arr.

>Edit: Mitch Faatz has pointed out in the comments that this may not be the best example. When testing, he determined that using the spread operator is three times slower than using concat. For larger data sets using the spread operator in this way would not be ideal.


### Math

We can also use math functions in conjunction with the spread operator. For this example, we’ll be using Math.max()
If you’re not familiar, Math.max() returns the largest of zero or more numbers:

```javascript
Math.max();
// -Infinity
Math.max(1, 2, 3);
// 3
Math.max(100, 3, 4);
// 100
```

Without the spread operator, the easiest way to use Math.max() on an array is to use .apply()

```javascript
var arr = [2, 4, 8, 6, 0];
function max(arr) {
  return Math.max.apply(null, arr);
}
console.log(max(arr));
// 8
```

Yeah, that’s really annoying.

Now take a look at how we do the same exact thing with the spread operator. Instead of having to create a function and utilize the apply method to return the result of Math.max() , we only need two lines of code:


```javascript
var arr = [2, 4, 8, 6, 0];
var max = Math.max(...arr);
console.log(max);
// 8

```


### String to Array
Bonus example: Use the spread operator to simply convert a string to an array of characters


```javascript
var str = "hello";
var chars = [...str];
console.log(chars);
// ['h', 'e',' l',' l', 'o']
```

Pretty cool huh? The spread operator walks through each character in the str string and assigns it to our new chars array.
