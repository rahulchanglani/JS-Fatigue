null and undefined
==================

```
null !== undefined
```

### Null

```Null means an empty or non-existent value. Null is assigned, and explicitly means nothing.```

```javascript
var test1 = null;
console.log(test1);
// null
```

null is also an object. Interestingly, this was actually an error in the original JavaScript implementation:

```javascript
console.log(typeof test1);
// object
```

### Undefined

```Undefined means a variable has been declared, but the value of that variable has not yet been defined.```

 For example:
```javascript
var test2;
console.log(test2);
// undefined
```

Unlike null, undefined is of the type undefined:
```javascript

console.log(typeof test2);
// undefined
```
Here are a two other examples of how you would get an undefined variable in JavaScript:
Declare a variable then assign undefined to it:
```javascript

var test = undefined;
console.log(test);
// undefined

```
Looking up non-existent properties in an object:

```javascript
var test = {};
console.log(test.prop);
// undefined
```

## Summary

Here are the quick facts:
>1. null is an assigned value. It means nothing.
>2. undefined means a variable has been declared but not defined yet.
>3. null is an object. undefined is of type undefined.
>4. null !== undefined but null == undefined.