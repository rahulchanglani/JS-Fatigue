Regular expressions
===================

Not too long ago regular expressions were terrifying to me — to my untrained eye they were nothing more than large strings of gibberish. Unfortunately, I let those strings of ‘gibberish’ scare me for far too long. When I finally decided to make the plunge and learn what regex was all about, I was surprised at how simple the basic concepts were to learn.

Don’t make the same mistake I did. Yes, regex can be very complex, but it’s actually pretty easy to learn the basics. So, that’s why I’m here. Jump in and learn basic regex with me… (and don’t forget to follow me on Medium!)

In JavaScript, a regular expression is simply a type of object that is used to match character combinations in strings.

### Make your first Regular Expression

There are two ways to construct a regular expression, by using a regular expression literal, or by using the regular expression constructor. Below, you’ll see an example of each of the methods. Each represent the same pattern — the character ‘c’ followed by ‘a’ followed by ‘t’.

```JavaScript
// Regular Expression Literal - Uses slashes ( / ) to enclose
var option1 = /cat/;
// Regular Expression Constructor
var option2 = new RegExp("cat");
```

As a general rule, if your regular expression will remain constant, i.e. your expression will not be changing, it is best to use a regex literal. If your regular expression will be changing, or relying on other variables, it is better to use the constructor method.

### RegExp.prototype.test()

Remember when I said a regular expression is an object? This means they have a number of methods we can use. The most basic method is test, which returns a Boolean:
* True: the string contains a match of the regex pattern
* False: no match found

```JavaScript
console.log(/cat/.test(“the cat says meow”));
// true
console.log(/cat/.test(“the dog says bark”));
// false
```

### Basic Regex Cheat Sheet

Honestly, the trick to learning regular expressions is memorizing the common character groups and symbols. I strongly encourage you to take a few hours and memorize the table below, then come back and we’ll continue learning. If you prefer to learn as you read, I’ll still introduce everything as we go, you just may have to refer back here from time to time.

#### Symbols

* __.__ — (period) Matches any single character, except for line breaks.
* __*__ — Matches the preceding expression 0 or more times.
* __+__ — Matches the preceding expression 1 or more times.
* __?__ — Preceding expression is optional (Matches 0 or 1 times).
* __^__ — Matches the beginning of the string.
* __$__ — Matches the end of the string.


#### Character groups

* __\d__ — Matches any single digit character.
* __\w__ — Matches any word character (alphanumeric & underscore).
* __[XYZ]__ — Character Set: Matches any single character from the character within the brackets. You can also do a range such as [A-Z]
* __[XYZ]+__ — Matches one or more of any of the characters in the set.
* __[^A-Z]__ — Inside a character set, the ^ is used for negation. In this example, match anything that is NOT an uppercase letter.


#### Flags:

There are five optional flags. They can be used separately or together and are placed after the closing slash. Example: /[A-Z]/g I’ll only be introducing 2 here.
* g — Global search
* i — case insensitive search


#### Advanced

* (x) — Capturing Parenthesis: Matches x and remembers it so we can use it later.
* (?:x) — Non-capturing Parenthesis: Matches x and does not remembers it.
* x(?=y) — Lookahead: Matches x only if it is followed by y.


### Test()-ing our Learning:

Before jumping into the projects below, lets test some of the above concepts out. Say we want to test a string for any numeric digits. We can use \d to accomplish this.

```JavaScript
console.log(/\d/.test('12-34'));
// true
```

The above returns true as long as there is at least one numeric digit in the string. What if we want to match the format though? We can use multiple \d characters to define a format:

```JavaScript
console.log(/\d\d-\d\d/.test('12-34'));
// true
console.log(/\d\d-\d\d/.test('1234'));
// false
```

What happens if we don’t care how may digits are before and after the ‘ - ’ so long as there is at least one? We can use the + to match the \d one or more times:

```JavaScript
console.log(/\d+-\d+/.test('12-34'));
// true
console.log(/\d+-\d+/.test('1-234'));
// true
console.log(/\d+-\d+/.test('-34'));
// false
```

To simplify things, we can use parenthesis to group expressions together. Lets say we have a cat meowing and we want to match against that meow:

```JavaScript
console.log(/me+(ow)+w/.test('meeeeowowoww'));
// true
Woah. OK. Lets break that down. There’s a lot going on up here.
/me+(ow)+w/
m     => matching a single letter 'm'
e+    => matching the letter 'e' one or more times
(ow)+ => matching the letters 'ow' one or more times
w     => matching the letter 'w' once
'm' + 'eeee' +'owowow' + 'w'
```

As you can see above, when operators like + are used immediately after parenthesis, they affect the entire contents of those parenthesis.

Last thing we’ll go over before going into the projects. The ? operator. This makes the preceding character optional. As you’ll see below, both test cases return true because the ‘s’s have been deemed optional.

```JavaScript
console.log(/cats? says?/i.test('the Cat says meow'));
// true
console.log(/cats? says?/i.test('the Cats say meow'));
// true
```

I’ve also thrown in the /i flag above. This makes our search case-insensitive which is why ‘cats’ will still match with ‘Cats’


### Considerations & Tips

Because regular expressions are wrapped in slashes, if you ever want to search for a slash, you need to escape it with a backslash. The same is true for characters such as question marks which have special meaning. Here’s an example of how to search for each of these:

```JavaScript
var slashSearch = /\//;
var questionSearch = /\?/;
```

* \d is the same as [0-9]: Each match a digit character
* \w is the same as [A-Za-z0-9_]: Each matches any single alphanumeric character or underscore


### Project: Add spaces in CamelCase:

In this example we’re growing really, really tired of camelCase and need a way to add spaces between words. Here’s the example:

```JavaScript
removeCc('camelCase') // => should return 'camel Case'
```

With Regex, there’s a very easy solution. First, we need to search for all capital letters. We can easily do this with a Character set search and a global modifier:

```JavaScript
This will match the C in 'camelCase'
/[A-Z]/g
```

But now, how do we add a space prior to the C?

We need to use Capturing Parenthesis! Capturing parenthesis allow us to match a value, and remember it, so that we can use it later!

```JavaScript
Use capturing parenthesis to remember our matched capital letter
/([A-Z])/

Access the captured value later with
$1
```

Above, you’ll see we use $1 to access the captured value. As an aside, if we had two sets of capturing parenthesis, we would use $1 and $2 to reference the captured values, and so on for the number of capturing parethesis.

Note, if you need to use parenthesis, but don’t need to capture the value, you can use non-capturing parenthesis: (?:x) In this example, the x is matched, but is not remembered.

Alright, back to the task at hand. How do we implement the capturing parenthesis? With the .replace() String method! As the second argument, we insert '$1' It’s important to use quotes here.

```JavaScript
function removeCc(str){
  return str.replace(/([A-Z])/g, '$1');
}
```

Hmmm, but wait, it doesn't work? Lets look at the code again. We’re capturing our capital letter, then simply replacing it with the same captured letter! We need to add in our space still! Inside our quotes we insert a space followed by our $1 variable The result is a space after each capital letter

```JavaScript
function removeCc(str){
  return str.replace(/([A-Z])/g, ' $1');
}

removeCc('camelCase') // 'camel Case'
removeCc('helloWorldItIsMe') // 'hello World It Is Me'
```

## Project: Remove Capital Letters:

Now we have a string with a bunch of funky capital letters. Can you figure out how to remove them? This one is similar, but a little trickier than the last project. Take a minute and try and figure it out, then read on..

First, we need to select all of the capital letters. This is similar to above, a character set search with the global modifier:

```JavaScript
/[A-Z]/g
```

We’ll be using the replace method again, but this time how to we make a character lower case?

```JavaScript
function lowerCase(str){
  return str.replace(/[A-Z]/g, ???);
}
```

Here’s a hint: With replace() you can specify a function as the second parameter.

We’re going to use an arrow function to un-capitalize the matched value. When using a function with replace() the function will be invoked after the match has been performed, and the function’s result will be used as the replacement string. Better yet, if the match is global and multiple matches are found, the function will be invoked for each match that is found.

```JavaScript
function lowerCase(str){
  return str.replace(/[A-Z]/g, (u) => u.toLowerCase());
}

lowerCase('camel Case') // 'camel case'
lowerCase('hello World It Is Me') // 'hello world it is me'
```

### Project: Capitalize First Letter:

Alright, you have all the knowledge to do this. Look at the below code snippet and try to solve it yourself before reading on:

```JavaScript
capitalize('camel case') // => should return 'Camel case'
```

Did you get it? If not, that’s OK! I’ll show you how…

Once again we’re going to use an arrow function in our replace() method. This time however, we only need to search for the very first character in our string. Recall that this is what ^ is used for.

Let’s look at ^ in depth for a second. Recall this example from earlier:

```JavaScript
console.log(/cat/.test('the cat says meow'));
// true
```

When we add in the ^ the function no longer returns true due to the fact that ‘cat’ is not at the beginning of the string:

```JavaScript
console.log(/^cat/.test('the cat says meow'));
// false
```

We want our ^ to apply to any lowercase character at the beginning of our string, so we’ll add it directly before our character set [a-z]. This will target only the first character if it is a lowercase letter.

```JavaScript
/^[a-z]/
```

Notice that we’re no longer using the global modifier as we only want one match. Now, we can plug our regular expression into our replace method and add in an arrow function as the second argument:

```JavaScript
function capitalize(str){
  return str.replace(/^[a-z]/, (u) => u.toUpperCase());
}

capitalize('camel case') // 'Camel case'
capitalize('hello world it is me') // 'Hello world it is me'
```

### Project: Keep Learning

That’s the end of this article, but keep learning. Some project ideas:

>1. Can you combine the prior three functions into one function that turns a camelCase string into a regular sentence?
>2. Can you add a period at the end of the string?
>3. Reverse everything! Turn a string into a camelCase Hashtag
