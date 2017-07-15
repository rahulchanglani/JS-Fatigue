Immediately-Invoked Function
============================

Function Declaration vs. Function Expression
Before we can learn why an IIFE is useful, we need to first understand exactly what it is. Let’s start all the way at the beginning by seeing what a typical function declaration looks like:
**FUNCTION DECLARATION**
function doSomething(){
  // ...do something...
};
Alright, pretty standard stuff - the keyword function followed the name of the function doSomething then by () and {}
The other way to create a function is through a function expression. Let’s look at the syntax below:
**FUNCTION EXPRESSION**
var doSomething = function(){
  // ...do something...
};
Visually, the main difference between our declaration and expression is somewhat small. And in fact, we would still invoke both of these functions in the same way — using the function name followed by parenthesis: doSomething(); . So what’s the catch?
The catch is a little complicated. You see, when the JavaScript Parser encounters the function keywords it usually assumes we’re writing a function declaration, unless we explicitly tell it that we’re not.
This is important, and helps shape the syntax of an IIFE. More on that in a second…
Immediately-Invoked Function Expression Syntax
It’s finally time to see what an Immediately-Invoked Function Expression looks like! As you can see below, it looks like a typical function declaration, except it’s wrapped in parenthesis and has a second set of parenthesis at the end:
(function(){
  // ...do something...
})();
Lets examine the two key aspects of an IIFE — one at a time. First, we’ll examine the enclosing parenthesis (shown below in blue)

This is where our earlier discussion of declaration vs. expression comes into play. Remember, JavaScript usually assumes we’re writing a function declaration when it encounters the function keyword. This is important because if you were to try to create an IIFE without the enclosing parenthesis, JavaScript thinks you’re attempting to make a function declaration but that you accidentally omitted the name of the function, and therefor a syntax error is thrown:
function(){ /*...do Something...*/ }();
// SyntaxError: Unexpected token (
As I mentioned above, when you type that in, JavaScript thinks you meant to create a function declaration:
// Did you mean to type this?
function doSomething(){ /*...do Something...*/ }();
But that’s not what we want. Luckily, this is where function expressions come into play.
By wrapping our function in parenthesis, we tell the parser to parse our JavaScript as a function expression, and not a function declaration. This allows our code to compile without any errors!
Awesome! Now we know what the first set of parenthesis in an IIFE do, what about the enclosing parenthesis at the end of our IIFE? (shown below in red)

As you may or may not know, parenthesis are used to invoke functions. So in our example above, a pair of parenthesis immediately after our function declaration will immediately invoke the function.
Why does this happen? Lets look at a simple example. Open up your Chrome Developer Console (Windows: Ctrl + Shift + J)(Mac: Cmd + Option + J) and type the following function declaration into your console:
function speak(){
    console.log('hello');
};
Now, you can invoke your function by simply typing speak() into the console. In return, 'hello' will be logged back to you. But, what happens if omit the parenthesis and just type speak ?
speak();
// 'hello'
speak;
//  function speak(){
//    console.log('hello');
//  }
Without the parenthesis, the function is never invoked, and thus the function definition is returned instead. That’s some pretty cool stuff.
Now it should be fairly obvious that by including the () at the end of our function expression, it will invoke the IIFE immediately.
The Why.
Awesome! We know exactly why an IIFE looks the way it does — and also what the code is doing: Creating a function expression and then invoking it immediately. Now we can answer the most important question, why?
Why would we use an IIFE instead of just creating a function and invoking it right afterwards?
Privacy.
In JavaScript, variables are scoped to their containing function. This means that they can’t be accessed outside of the function. Here’s a simple example:
(function(){
  var superSecret = 195;
})()
console.log(superSecret);
//  Uncaught ReferenceError: superSecret is not defined
We can’t access the superSecret variable outside of the IIFE. All of the code within our IIFE stays within the private scope of our function.
But wait, why would you just create a named function and invoke it? That would create the same result?
Yes, but with consequences: Creating a named function pollutes the global name space. It also means the named function is hanging around. With the function hanging out, oh-so readily available, it could accidentally be invoked again. Our IIFE isn’t named and therefor can’t accidentally be called later — avoiding any potential security implications.
