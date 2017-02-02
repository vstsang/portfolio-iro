---
layout: post
title: Immediately Invoked Function Expressions
---
As a JavaScript apprentice, I have hit plenty of bumps along the way during my learning journey. In the great tradition of [rubber ducking](https://en.wikipedia.org/wiki/Rubber_duck_debugging), I hope to explain some of these concepts in a series of blog posts that may help other fellow learners, but also to solidify my own understanding. 

Here is the short list of topics that I plan to start with:
* Immediately Invoked Function Expressions
* What I've learned after 4 months of JavaScript with Bloc
* Class Responsibility Collaborator in 5 minutes

Today we will begin with Immediately Invoked Function Expressions (IIFE).

## Immediately Invoked Function Expressions

Sometimes also referred to as Self Invoking Function Expression, it is probably not the best term, but the concept itself is self explanatory. Most JavaScript students would learned function expressions within the first few lessons. 

Instead of defining a function through a named declaration, function expression is defined through a variable as part of a larger expression syntax. Function expression can also be created in group context by wrapping parenthesis around the function definition. Both formats can either be named or anonymous.

{% highlight javascript %}

// An anonymous function expression assigned to a variable
var foo = function () {
  console.log("bar");
}

// An anonymous function expression in group context
(function () {
  console.log("bar");
}) 

{% endhighlight %}

Once we have established what is a function expression, it is straight forward that we can call or invoke the function immediately by placing parentheses after it, just as any normal function calls would.

{% highlight javascript %}

// Immediately Invoked Function Expression with variable assignment
var foo = function () {
  console.log("bar");
}();

// Immediately Invoked Function Expression
(function () {
  console.log("bar");
})() 

{% endhighlight %}

Since in most cases IIFE are used with anonymous function expressions, we'll focus our example on that. 

## Why and when IIFE should be used

There are a few scenarios when Immediately Invoked Function Expressions are used; such as keeping the global variable scope clean in function libraries (e.g. jQuery) by keeping the function anonymous, another is using them within `if` statements (functions normally cannot be declared within conditionals). However by far the most common uses are closures in for loops to lock in the correct value.

>_Note: In my research for this post, IIFE is also used extensively with Module Patterns. It is a way to emulate classes in JavaScript with private methods using object literals. However this is beyond my learning level at the moment and the scope of this post._

Because closure stores its outer values by references only and not by value, this creates an issue within loops by getting the value at the end of the loop and not during the loop. Therefore by immediately invoking the function as soon as it is created, this problem is avoided.

Borrowing a common closure for loop example from [Angular University](http://blog.angular-university.io/really-understanding-javascript-closures/) below, the code should expect a return counter value of 0, 1, 2; but instead it outputs 3, 3, 3.

{% highlight javascript %}

// define a function that increments a counter in a loop
function closureExample() {

    var i = 0;

    for (i = 0; i< 3 ;i++) {    
        setTimeout(function() {
            console.log('counter value is ' + i);
        }, 1000);
    }

}
// call the example function
closureExample();  

// console
->"counter value is 3"
->"counter value is 3"
->"counter value is 3"

{% endhighlight %}

This can be fixed simply using IIFE and passing the loop counter as argument. This in turn will reference the incrementing loop counter every time the IIFE is called.

{% highlight javascript %}

// define a function that increments a counter in a loop
function closureExample() {

    var i = 0;
    
    for (i = 0; i< 3 ;i++) {    
      (function closure(i) { 
        setTimeout(function() {
          console.log('counter value is ' + i);
        }, 1000);
      })(i);
    }

}
// call the example function
closureExample();

// console
->"counter value is 0"
->"counter value is 1"
->"counter value is 2"

{% endhighlight %}

## Conclusion
Immediately Invoked Function Expressions may appear a simple as a concept, but knowing when it should be used is the key. It is commonly used in function libraries such as jQuery to keep a clean global namespace. However it is most often used for closures within for loops to ensure the correct counter value is referenced.


## Further Reading
* [Ben Alman on Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)
* 	[Powerful Uses of Immediately Invoked Function Expressions](http://javascriptissexy.com/12-simple-yet-powerful-javascript-tips/)