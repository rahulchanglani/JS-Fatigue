Recursion
==============

_Recursion is simply when a function calls itself._

Lets jump right in and take a look at probably the most famous recursion example. This example returns the factorial of a supplied integer:

```javascript
function factorial(x) {
  if (x < 0) return;
  if (x === 0) return 1;
  return x * factorial(x - 1);
}
factorial(3);
// 6
```

Woah. It’s Okay if that makes no sense to you. The important part is happening on line 4: return x * factorial(x — 1);. As you can see, the function is actually calling itself again ( factorial(x-1) ), but with a parameter that is one less than when it was called the first time. This makes it a recursive function.

```
Before I break down that code example any further, it’s important you understand what factorials are.
```

To get the factorial of a number you multiply that number by itself minus one until you reach the number one.

Example 1: The factorial of 4 is 4 * 3 * 2 * 1, or 24.

Example 2: The factorial of 2 is just 2 * 1, or 2.

Awesome, now that our High School Math lesson is over, we can return to the good stuff!


### The three key features of recursion

All recursive functions should have three key features:

#### A Termination Condition

Simply put: ```if(something bad happend){ STOP };``` The Termination Condition is our recursion fail-safe. Think of it like your emergency brake. It’s put there in case of bad input to prevent the recursion from ever running. In our factorial example, ```if (x < 0) return;``` is our termination condition. It’s not possible to factorial a negative number and thus, we don’t even want to run our recursion if a negative number is input.


#### A Base Case

Simply put: ```if(this happens) { Yay! We're done };``` The Base Case is similar to our termination condition in that it also stops our recursion. But remember, the termination condition is a catch-all for bad data. Whereas the base case is the goal of our recursive function. Base cases are usually within an if statement .In the factorial example, if (x === 0) return 1; is our base case. We know that once we’ve gotten x down to zero, we’ve succeeded in determining our factorial!

#### The Recursion

Simply put: Our function calling itself. In the factorial example, ```return x * factorial(x — 1);``` is where the recursion actually happens. We’re returning the value of the number x multiplied by the value of whatever factorial(x-1) evaluates to.

#### All Three Together

Now we still have no idea how our factorial example works, but ideally it makes more sense:

```javascript
function factorial(x) {
  // TERMINATION
  if (x < 0) return;
  // BASE
  if (x === 0) return 1;
  // RECURSION
  return x * factorial(x - 1);
}
factorial(3);
// 6
```

### Factorial Function Flow

Lets examine exactly what happens when we call our Factorial Function:

>1. We first call our function passing in the value of 3.

```javascript
factorial(3);
```

>2. This results in the function being run. Both if statements fail and our recursion line runs. We return the integer 3 multiplied by the value of factorial(3-1).

```javascript
return 3 * factorial(2);
```

>3. When factorial(2) is run, both if statements fail again and recursion occurs. We return the integer 2 multiplied by the value of factorial(2-1).

```javascript
return 2 * factorial(1);
```

>4. When factorial(1) is run, both if statements fail again and recursion occurs. We return the integer 1 multiplied by the value of factorial(1-1).

```javascript
return 1 * factorial(0);
```

>5. When factorial(0) is run, something different happens. Zero is our base case, so that if statement passes and our function returns 1.

```javascript
if (x === 0) return 1;
```

**Now that our function has finally returned, everything will ‘unwind’. This is because recursion is simply a group of nested function calls. With nested functions, the most inner nested function will return first.**

```This is the important part to understand. Read over this a few times if you don’t understand it at first.```

```javascript
factorial(0) returns 1
factorial(1) returns 1 * factorial(0), or just 1*1
factorial(2) returns 2 * factorial(1), or just 2*1*1
factorial(3) returns 3 * factorial(2), or just 3*2*1*1
return 1 * 1 * 2 * 3
// 6
```

Did you follow that? Here is the same explanation structured differently:

```javascript
factorial(3) returns 3 * factorial(2)
factorial(2) returns 2 * factorial(1)
factorial(1) returns 1 * factorial(0)
factorial(0) returns 1
// Now that we've hit our base case, our function will return in order from inner to outer:
factorial(0) returns 1                 => 1
factorial(1) returns 1 * factorial(0)  => 1 * 1
factorial(2) returns 2 * factorial(1)  => 2 * 1 * 1
factorial(3) returns 3 * factorial(2)  => 3 * 2 * 1 * 1
// 3 * 2 * 1 * 1 = 6
```

It’s okay if you still don’t understand what’s happening. Lets move on to another example to try and clear up some confusion.

#### Lets look at a second example

We’re going a completely different route with this second example. This is another popular example on the internet and it has to do with a reversing a string.
**Please note that this is not the most efficient way to reverse a string in JavaScript. There are much faster ways. It’s just a common internet example of recursion so I wanted to cover it as well.**

Here’s what the code looks like:

```javascript
function revStr(str){
  if (str === '') return '';
  return revStr(str.substr(1)) + str[0];
}
revStr('cat');
// tac
```

Instantly you can (ideally) notice a few things:

>1.str === "" is our base case. When our string has no characters in it, we’ve succeeded.
>2.return revStr(str.substr(1)) + str[0]; is where the recursion magic happens.
>3.There is no termination case. That’s because in this instance our base case is our termination case. We can’t get a string that has negative characters. so as long as only strings are entered into our function we will be fine.


#### Let’s break it down line by line

In this example we’re reversing the string cat. We start with a call to our function and passing in the string cat:

```javascript
revStr('cat');
```

Our Recursive case is run.

In JavaScript the substr() method returns a string beginning at the specified location. Thus ‘cat’.substr(1) === ‘at’
str[0] gives us the character at that index in the string. Thus cat[0] === 'c'

```javascript
return revStr(str.substr(1)) + str[0];
// SAME AS
return revStr('at') + 'c'
```

Our recursive case is run again:

```javascript
return revStr(str.substr(1)) + str[0];
// SAME AS
return revStr('t') + 'a'
```

Our recursive case is run one final time:

```javascript
return revStr(str.substr(1)) + str[0];
// SAME AS
return revStr('') + 't'
```

This time our base case runs, and the function returns a blank string:

```javascript
if (str === '') return '';
```
Now that our function has returned, everything will ‘unwind’ and return in order:

```javascript
return ‘’ + ‘t’ + ‘a’ + ‘c’
// tac
```

#### Break it down even more

For good measure, here’s the step by step again:

```javascript
revStr('cat')  returns revStr('at') + 'c'
revStr('at')   returns revStr('t') +  'a'
revStr('t')    returns revStr('') +   't'
revStr('')     returns               ''
```

Now, remember, these are all nested function calls. When you have nested function calls, the most inner nested function returns first. This initiates the ‘unwinding’ as they return in order

```javascript

revStr('')     returns                ''  => ''
revStr('t')    returns revStr('') +   't' => '' + 't'
revStr('at')   returns revStr('t') +  'a' => '' + 't' + 'a'
revStr('cat')  returns revStr('at') + 'c' => '' + 't' + 'a' + 'c'
// tac
```