---
layout: post
title: Understanding Lambdas
---

Before we dive into the specifics of using lambda expressions in Java, and why they are beneficial let’s start at the beginning with Lambda Calculus. Don’t be afraid by the name Lambda Calculus, it’s just a different way of thinking of functions! This will be a high-level over-view of Lambda Calculus if you want to dive deeper into the math I would recommend this post [lambda calculus for dummies](http://palmstroem.blogspot.co.nz/2012/05/lambda-calculus-for-absolute-dummies.html).

Okay, I’m sure you're used to the traditional idea of math functions 

$$f(x) = 2x + 1$$



$$g(x) = 3x + 2$$

These functions have names and this is how we identify them. Remember in math class you were told to use functions \\(f(x)\\) and \\(g(x)\\) to solve blah. Well in Lambda Calculus functions don’t have names they’re anonymous. Well if a function doesn’t have a name how can it be identified? Let’s look at how the above functions would be written in lambda calculus.

(λx.2x + 1)

(λx.3x + 2)

Now we’ll look at the syntax individually.

λ → (lambda. Get it, lambda calculus!) this defines our function
 
x → this is the input variable for the function

2x + 1→ the function definition     

The above functions can no longer be identified by names, but instead the functions are now identified by their function definitions. With the above information you can think of lambda expressions as anonymous (nameless) functions in Java. 

Why should you learn lambda expressions in Java 8
---
You might be wondering what’s the point of learning lambda expressions if the same thing can be accomplished with traditional functions that you’ve been using for years. For a few high level reasons lambda expressions reduce the total amount of code needed for a function, help you read modern java code, and improve your the codebase readability. Let’s look at some examples.
The syntax for a lambda expression in java is 
    (argument) -> (body)
Let’s look at a simple use case printing the contents of a list
