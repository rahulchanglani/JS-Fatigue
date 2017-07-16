Short Hand Operators
====================

### Variables with equal values

```javascript
// LONG WAY
var a = 1;
var b = 1;
var c = 1;
// SHORT WAY
var a = b = c = 1;
```

### Variables with different values

```javascript
// LONG WAY
var d = 2;
var e = 3;
var f = 4;
// SHORT WAY
var d = 2, e = 3, f = 4;
```

### Bonus: Shorthand Assignment Operators

If you’re adding, subtracting, multiplying, dividing, or remaindering two values, there’s no need to type out the entire equation. Use these shorthands to save time and code:

Long way:

```javascript
var x = 1;
var y = 2;
x = x + y;
console.log(x);
// 3
```

Shorthand:
```javascript
var x = 1, y = 2;
x += y;
console.log(x);
// 3
```

Other Shorthands:
```javascript
x = x + y    ->    x += y
x = x - y    ->    x -= y
x = x * y    ->    x *= y
x = x / y    ->    x /= y
x = x % y    ->    x %= y
```