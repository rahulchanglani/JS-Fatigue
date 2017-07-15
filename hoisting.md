Hoisting
=========

When Javascript engine builds code, it uses a technique called Hoisting which divides the execution into 2 passes: The first pass is called **declaration** where it scans for all variables and function declarations ( Yet, they are treated differently ) and creates identifiers for all of them. Then the second pass is the **execution** pass.

Suppose we have this code snippet
```JavaScript
(function my_function() {
  console.log(bar); // undefined
  var bar = 1;
})()
```


The first pass ( declaration ) makes the above code look like the following:
```JavaScript
(function my_function() {
  var bar;
  console.log(bar); // undefined
  bar = 1;
})()
```

### Hoisting and Function

Hoisting works differently with function declarations since the first pass ( declaration ) scans the function body along with its name. Below is an example on what it means

```JavaScript
(function () {

  console.log(1+two()); // 3

  function two(){
    return 2;
  }
})()
```

Yet, function expressions are hoisted the same way as variables where the first pass ( declaration ) scans only the function name. Below is an example showing how hoisting works with function expressions

```JavaScript
(function () {

  console.log(three); // undefined
  console.log(1+three()); // TypeError: three is not a function

  //anonymous function expression
  var three = function (){
    return 3;
  }

})()
```

```JavaScript
(function () {
  console.log(four); // undefined
  console.log(1+four()); // TypeError: four is not a function

  //named function expression
  var four = function four() {
    return 4;
  }

})()
```

### Hoisting and let ( ES6 )

The let statement declares a block scope local variable, optionally initializing it to a value.
In ES6, let bindings are not subject to Variable Hoisting, which means that the first pass ( declaration ) doesn’t scan variable declared with let. Referencing the variable in the block before the initialization results in a ReferenceError (contrary to a variable declared with var, which will just have the undefined value).

```JavaScript
(function () {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError: foo is not defined
  var bar = 1;
  let foo = 2;
})()
```

### Hoisting and const ( ES6 )

Constants are block-scoped, much like variables defined using the let statement. The value of a constant cannot change through re-assignment, and it can't be redeclared.

In ES6, and much like let, constbindings are not subject to Variable Hoisting, which means that the first pass ( declaration ) doesn’t scan variable declared with const. Referencing the variable in the block before the initialization results in the same error as if we used let (contrary to a variable declared with var, which will just have the undefined value).

```JavaScript
(function () {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError: foo is not defined
  var bar = 1;
  const foo = 2;
})()
```

### Hoisting and class declaration ( ES6 )

The class declaration creates a new class with a given name using prototype-based inheritance.
Unlike function declarations, class declarations aren’t hoisted.

```JavaScript
(function () {

  console.log(Dog); // ReferenceError: Dog is not defined

  class Dog{
    constructor(name){
      this.name = name;
    }
  }
})()
```

## Conclusion

Hoisting works with variable declarations using var but it doesn’t work when the variables were declared with let or const.
Function declarations are treated in a special way where the function name and body both get hoisted.
