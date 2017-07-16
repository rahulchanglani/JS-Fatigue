Pass by Value & Pass by Reference
====================================


When googling around I wasn’t able to find any material that I felt explained it as simply as the concept really is. Rather than complain I decided to take action and explain this concept in the simplest way possible for all future Javascripters (<< just invented that) to reference (no pun intended).

In order to understand passing by reference and by value you must first be aware of primitives and objects. Primitive values in Javascript are aptly named and there are five of them. They are the most basic values one can think of including undefined, null, boolean, string, and numbers. Primitive values are passed by value which is the first part we’ll dive into.

### Pass By Value (primitives)

Interestingly enough the equals operator plays a huge role in how value and reference work. When creating a variable the equals operator will notice that you are assigning that variable a primitive value (or object) and act accordingly. I’m going to refer to the picture below in the following text to help give a visualization of what is going on..

![Pass by Value](https://cdn-images-1.medium.com/max/800/1*nZpsZjDatEzXw5PZP2hGUA.png "Pass by val")

When assigning a variable (a) a primitive value the equals operator sets up a location (address) in memory (represented by 0x001 in the picture) to store the information and points the variable (a) to that address. When you create a new variable (b in this case) and assign it the value of another variable (a) the equals operator creates ANOTHER spot in memory separate from the original variable (represented by 0x002 in the picture) and places of copy of (a) in the new variables spot in memory. So, by value copies the value of the original variable (a) into two separate spots in memory. Being unaware of this can cause some interesting interactions, lets take a look below:

```javascript
let a = 5 
let b = a

console.log(a) // => 5
console.log(b) // => 5

a = 1

console.log(a) // => 1
console.log(b) // => 5
```


In the above we assign variable (a) the value of 5. The equals operator notices the value is a primitive and creates a new location in memory, points (a) to the address, and fills it with the value 5. When we create variable (b) and assign it the value of (a) the equals operator notices we’re dealing with a primitive value (5 in this case) and creates a NEW location in memory, points (b) to the NEW address, and fills it with a copy of (a)’s value (5). The console.log() of each variable prints what we would expect (5) at this point. However, when we change the value of (a) to 1 and console.log both variables (a) prints 1 as expected, but (b) is still equal to 5. Why? Because as discussed earlier when we assigned (b) to equal (a) a new location in memory was created, the equals operator did not simply have (b) point to the same spot in memory that (a) does, (b) was given its own location filled with the value of (a) at that time (5). So when (a) was changed (b) does not follow suit because it’s pointing to it’s OWN spot in memory, it has no idea that (a)’s value has changed since that is an entirely separate address in memory.

In sum, by value copies the value into two separate spots in memory effectively making them entirely separate entities despite one initially being set equal to the other.


### Pass by Reference (objects)

Passing by reference relates to objects in Javascript (ALL objects including functions). Like before im going to utilize the picture below as a visual representation of what we’ll be discussing.

![Pass by Reference](https://cdn-images-1.medium.com/max/800/1*t1Jjp3moTD1KCA75Kd_PSA.png  "Pass by Ref")

As you can see above when a variable (a) is set equal to an object the equals operator identifies that the value is an object, creates a new location in memory, and points (a) to the address (represented by 0x001). When we create a new variable (b) and assign it the value of variable (a) the equals operator knows we are dealing with an object and points it to the same address that (a) points to. Notice that no new location or object in memory is created (like in pass by value), rather variable (b) is simply pointed to the same address that variable (a) was pointed to. This too can lead to some interested interactions if you dont know how things work, lets take a look at the example below:

```javascript
let a = {language: "Javascript"}
let b = a

console.log(a) // => {language: "Javascript"}
console.log(b) => {language: "Javascript"}

a.language = "Ruby"

console.log(a) // => {language: "Ruby"}
console.log(b) // => {language: "Ruby"}
```

In the example above I create a variable (a) and set it equal to an object (in this case {langauge: “Javascript”}. The equals operator recognizes that the value is an object, creates a new spot in memory, and points (a) to it. I then create a new variable (b) and set it equal to (a). The equals operator identifies we are dealing with objects and points (b) to the same location in memory that (a) is pointed to. No new location or object in memory was created, rather both variables are pointing to the SAME location (address). So, when I mutate the the value of the key in variable (a) (in this case changing “Javascript” to “Ruby”) when I console.log the results 0f (a) and (b) they are the same since (b) points to the same location that (a) does.

In sum, ALL objects interact by reference in Javascript so when setting equal to each other or passing to a function they all point to the same location so when you change one object you change them all. This is a stark difference compared to pass by value.
