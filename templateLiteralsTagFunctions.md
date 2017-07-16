Template Literals and Tag Functions
=====================================

I hate click-bait titles as much as anyone else, but seriously, use this “One simple trick to improve your code readability instantly”.

>The back-tick is your friend.

Template Literals use enclosing back-ticks ( ` ) instead of single ( ‘ ) or double ( “ ) quotes.

__BOOM.__ You can now write a template literal:

```javascript
console.log(`Hello World`);
// "Hello World"
```

Alright, that was a crappy example. Lets add in a variable with ${}

```javascript
var name = 'World';
console.log(`Hello ${name}`);
// "Hello World"
```

A little better, but lets back up for a second:

What exactly is a “Template Literal”? Well a template is a “preset format” and a literal is a “value written exactly as it’s meant to be interpreted”. In JavaScript terms, a template literal is a way to concatenate strings while allowing embedded expressions and improving readability.

### Readability & Clean Code

Consider a scenario like this where we want to log out a person’s name and nickname. Using template literals means not only less code, but much higher readability:


```javascript
var p = {
  name: 'Jackson',
  nn: 'Jak',
};
// STRING CONCATENATION
console.log('Hi, I\'m ' + p.name + '! Call me "' + p.nn + '".');
// TEMPLATE LITERALS
console.log(`Hi, I'm ${p.name}! Call me "${p.nn}".`);
// "Hi, I'm Jackson! Call me 'Jak'."
```

In this example there are two annoying features string concatenation that stick out to me. Escaping the apostrophe with a back-slash, and trying to figure out what is going on at the end of the string with the double and single quotes. Both of these concerns are alleviated with template literals, and we’re left with a much cleaner line of code.
Line breaks are another area where template literals shine:

```javascript
// STRING CONCATENATION
console.log("Dear Mom,\n" +
"Hope you are well.\n" +
"\tLove, your son")
// TEMPLATE LITERALS
console.log(`Dear Mom,
Hope you are well.
    Love, your son`);
// Dear Mom,
// Hope you are well.
//     Love, your son
```

With template literals, any new line characters, tabs, spaces, etc. inserted in the source become a part of the string. This can be both a blessing and a curse, but in terms of readability is definitely a plus.
One more readability example. Consider adding expressions to a string:

```javascript
// STRING CONCATENATION
console.log('Three plus six is ' + (3 + 6) + '.');
// TEMPLATE LITERALS
console.log(`Three plus six is ${3 + 6}.`);
// "Three plus six is 9."
```

### Tagged Templates

Tagged templates is another use case for Template Literals. A tagged template is a function call that uses a template literal from which to get its arguments.

```javascript
// TYPICAL FUNCTION
function greet(){};
// TAG FUNCTION
greet`I'm ${name}. I'm ${age} years old.`
```

As you can see, a tag function is simply the function name followed by a template literal. But a tag function is purely syntactic sugar. Using the above Tag function is equivalent to this:


```javascript
// TAG FUNCTION
greet`I'm ${name}. I'm ${age} years old.`
// EQUIVALENT FUNCTION
greet(["I'm ", ". I'm ", " years old."], name, age)
```

As you can see, a tag function is an easy way to pass a template literal into a function as its arguments. The tag function breaks down the template literal into an array of arguments. The first argument will be an array of string values. Any additional arguments will be variables in the order they are encountered. Here’s a full example to hopefully drive this idea home:

```javascript

var name = 'Brandon';
var age = 26;

function greet(){
  console.log(arguments[0]);
  // ["I'm ", ". I'm ", " years old."]

  console.log(arguments[1]);
  // Brandon

  console.log(arguments[2]);
  // 26
}
greet`I'm ${name}. I'm ${age} years old.`;
```

As you can see, the tag function uses and ‘breaks-down’ our template literal. But obviously this example isn’t the most useful. Lets look at one that is:

```javascript

var name = 'Brandon';
var age = 26;
function greet(arr, nameArg, ageArg) {
  console.log(arr[0] + nameArg + arr[1] + ageArg + arr[2]);
}
greet`Woah, ${name} is ${age}?`;
// "Woah, Brandon is 26?"

```

Pretty cool huh? When we invoke the greet function we pass in our template literal as the sole argument. The Tag functions breaks down our template literal into three separate arguments. The first argument is an array of our plain text. The remaining arguments are the template literal expressions in the order they appear. We can then access and arrange all of these arguments to produce the desired thread! In this case we sandwich our arguments in the middle of our array and viola — we get our text to say what we want it to say!
