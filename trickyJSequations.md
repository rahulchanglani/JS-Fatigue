Tricky JS expressions
====================

It’s about to get weird — and you’re going to want to follow along with me. You can open up your Chrome Developer Console with: (Windows: Ctrl + Shift + J)(Mac: Cmd + Option + J). This will allow you to type all of the following code into your browser so you can see in real time what is happening…

```javascript
3 + true == 4
```

I’m not kidding either. In JavaScript, when the plus operator is placed between a number and a boolean, the boolean is converted to a number.

If you remember back to my JavaScript Showdown article, false == 0 and true == 1. With this in mind, 3 + true is converted to 3 + 1 and thus we get the answer of 4.
```javascript
true + false
```
This follows the same logic as the above example. When the plus operator is placed between two booleans, the booleans are converted to numbers. Thus, true + false is converted to 1 + 0 and we get an answer of 1.
```javascript
‘4’ + 8
```
What happens when we add a string number to an actual number? When the plus operator is placed between to operands, and one is a string, it will convert the other number or boolean to a string and concatenate them.
By this logic: '4' + 8 becomes '4' + '8' and we get an answer of '48'.

```javascript
true + ‘4’
```
Similar to the above example, JavaScript will convert the boolean into a string value and concatenate. This becomes 'true' + '4' and the result is 'true4'.
```javascript
1 + 1 + ‘1’
```
Order of operations is important. In this instance, JavaScript evaluates the first + before anything else: 1 + 1 equals 2. Then we move on and add in the string value of '1'. Concatenation occurs and the result is '21'.
Here’s the chain of events:
```javascript
1 + 1 + '1'
  2   + '1'
       '21'
‘1’ + 1 + 1
```
What changes when we have our string value first instead of last? Remember the order of operations and work from left to right:

```javascript
'1' + 1 + 1
   '11' + 1
       '111'
```
string + number = string. So in this instance we’re left with one long string of '111'.

```javascript
-‘69’ + 69
```
What if we attempt to negate a string and then add a number? As you should know by now, without the negation, our answer would be '6969'. However, the negation changes things.
The minus sign before '69' is a unary minus operator that will actually convert the string to a number and make it negative. Thus, our equation becomes -69 + 69 which equals 0.
```javascript
-‘giddyup’ + 409
```
What if our unary minus operator is in front of a string that can’t be converted to a number? When JavaScript fails to produce a number, we are left with NaN (Not A Number).
