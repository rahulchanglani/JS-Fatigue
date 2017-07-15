Short-circuit Evaluation
========================

Learn how to use logical OR ( || ) to your advantage when assigning variables.

image via unsplash.com
Preface
Hey! I’m Brandon. I created codeburst.io and I write JavaScript tutorials and articles to help beginners better understand the inner workings of Web Development. If you have any questions about the article, leave a comment and I’ll get back to you, or find me on twitter @brandonmorelli. Lastly, when you’re ready to really dive into Web Development, Check out the Best Courses for Learning Full Stack Web Development
The 3 logical operators
There are three logical operators in JavaScript:
Logical AND &&
Logical OR ||
Logical NOT !
Today we’ll be exploring Logical OR ||. In JavaScript the Logical OR operator returns true if either operand is true:
true || true;
// true
true || false;
// true
false || false;
// false
Two important aspects of logical operators in JavaScript is that they evaluate from left to right, and they short-circuit.
What this means is that when JavaScript evaluates an OR expression, if the first operand is true, JavaScript with short-circuit and not even look at the second operand. In the example below the asterisks (****) indicate any value — it simply doesn’t matter what it is as JavaScript will never even read that side of the Logical OR.
true || ****
// true
So why is this important?
Because we can use this short-circuiting to our advantage! In the below example we have a person object that has a name and an age. We then want to console.log the job that our person has. The problem is, we don’t know if our object contains the job key. We can use the || and short-circuit evaluation to make this easy:
var person = {
  name: 'Jack',
  age: 34
}
console.log(person.job || 'unemployed');
// 'unemployed'
What we’ve done is console.log the value of person.job OR the default string of 'unemployed'. Since person.job doesn’t exist, we get undefined. Since undefined is a falsy value, JavaScript will instead go to the other side of the || and accept whatever value is there.
To make this example clearer, lets look at what happens if our person object includes a key for job:
var person = {
  name: 'Jack',
  age: 34,
  job: 'teacher'
}
console.log(person.job || 'unemployed');
// teacher
This time, person.job exists. Therefor, the equation short-circuits and the value of person.job (‘teacher’) is logged to the console instead.
Need a refresher on falsy values in JavaScript? Read JavaScript Showdown
One more example.
This is a popular snippet of code from stack overflow:
var a;
var b = null;
var c = undefined;
var d = 4;
var e = 'five';

var f = a || b || c || d || e;

console.log(f);
Look at the code above and think about what will be logged to the console. Got an idea? Scroll down for the answer…
Answer
Did you guess 4? Nice work! If not, that’s okay too, lets break it down:
var a; // undefined (falsy value)
var b = null; // null (falsy value)
var c = undefined; undefined (falsy value)
var d = 4; // number (NOT falsy)
var e = 'five'; // assigment short circuits before reaching here

var f = a || b || c || d || e;
a, b, c, all are assigned falsy values. When Logical OR sees a falsy value, it continues evaluating. Once it reaches variable d with a value of 4, the equation short-circuits and the value of 4 is saved to variable f.
