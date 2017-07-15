JS Showdown
============

JavaScript has two visually similar, yet very different, ways of testing equality. This is your high level overview of the differences.

### Triple equals (===)

The triple equal sign is the one you’re probably familiar with. It tests for strict equality between two values. Both the type and the value you’re comparing have to be exactly the same.
Examples of strict equality:

```JavaScript
3 === 3
// true (Both numbers, equal values)
'test' === 'test'
// true (Both Strings, equal values)
false === false
// true (Both Booleans, equal values)
```

Not strict equality:

```JavaScript
3 === '3'
// false (Number compared to String)
'hello' === 'goodbye'
// false (Different Strings)
false === 0
// false (Different type: Boolean vs Number)
```

### Double equals (==)

The double equals tests for **loose equality** and preforms type coercion. This means we compare two values after converting them to a common type.

Lets look at an examples to help this sink in. Remember above when we tested the strict equality of a string and number?

```JavaScript
3 === '3'
// false (Number compared to String)
3 == '3'
// true
```

When we compare the number 3 with the string '3' using loose equality, we get true — they are equal.
With double equals, JavaScript attempts to convert the values into a common type. In this example, JavaScript converts the string value of '3' into a number, then compares 3 and 3. Hence the true value.


### Falsy Values

How about another example:

```JavaScript
false === 0
// false (Different type: Boolean vs Number)
false == 0
// true
```

In this example, we see that when comparing with loose equality, false == 0 evaluates to true. This is more complex example, but has to do with the fact that in JavaScript, 0 is a falsy value.

In JavaScript there are only **six falsy** values.
* **false**
* **0** (zero)
* **“”** (empty string)
* **null**
* **undefined**
* **NaN** (Not A Number)

Comparing false, 0, and "" with loose equality (==) will result in equality:

```JavaScript
false == 0
// true
0 == ""
// true
"" == false
// true
```

```JavaScript
null and undefined are only equal to themselves:
null == null
undefined == undefined
null == undefined
// true
```

NaN isn’t equivalent to anything (not even itself!):

```JavaScript
NaN == null
NaN == undefined
NaN == NaN
// false
```

## Key Takeaways

### Triple Equals === is the victor.

Whenever possible, you should use triple equals to test equality. By testing the type and the value, you are always executing a true equality test.
Type coercion can have some crazy rules involved (as I’ve shown above). Unless you’re very familiar with JavaScript, double equals can lead to more trouble and headaches than it’s worth. Knowing the six falsy values can go a long way to determining why something is evaluating to true or false.
