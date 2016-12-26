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


~~~~ 
(argument) -> (body)
~~~~
Let’s look at a simple use case printing the contents of a list


~~~~
public static void main(String[] args) {
   List<String> avengersRoster = Arrays.asList("iron man", "captain america", "ant man", "spider-man", "black widow", "hulk", "thor");


   //old style
   for (String avenger : avengersRoster) {
       System.out.println(avenger);
   }

   //lambda expression
   avengersRoster.forEach(a -> System.out.println(a));
}
~~~~
{: .language-java}

With the old style three lines of code are needed just to print each avenger. By using a lambda expression you reduced the needed code to one line! 
Take a look at the actual lambda expression 

~~~~
a -> System.out.println(a)
it follows the lambda syntax: (argument) -> (body)
~~~~ 

Lambda Expression Syntax: 
---
+ A lambda expression can have zero, one , or many parameters
    + ~~~~  
    () -> System.out.println(“Hello World”) 
     a -> System.out.println(a) 
    (a, b) -> System.out.println(a + “ “ + b)
      ~~~~ 

+ The type of the parameter can be declared or it can be inferred from the context.
    + ~~~
    (int a) is the same as (a)
      ~~~

+ If more than one parameter is present the parameters are separated by commas and enclosed in parenthesis
    + ~~~
    (String boat, int boaty, float mcboatface)
    (boat, boaty, mcboatface)
      ~~~

+ The body of the lambda expression can have zero, one, or many expressions. If zero or many expressions exist you must use curly braces. Think of it like an if statement or for loop if only one line of code follows curly braces aren’t needed.
    + ~~~
    () -> {} this does nothing it takes no arguments and the body has no expression, but is still a valid lambda expression 
    (int x, int y) -> { return x + y; }  or 
    (int x, int y) -> return x + y; both are valid since the body only contains one expression 
    (List<Integer> x) -> {int min = x.get(0); for (int i = 1; i < x.size(); i++) if (x.get(i) < min) min = x.get(i); System.out.println(min); };
      ~~~
In the last example multiple expressions exist in the body, so curly braces are required in this instance. Let’s break down the body to find out what the lambda expression does. The first thing we see in the body is a variable getting initialized with the first element in the array. Based on the variable name and seeing the for loop you can guess this expression just finds the minimum value in an array. 

      
More Examples:
---

A common use case for lambdas is using them as a comparator for the sort method. Let’s do some sorting without lambdas first! 

~~~
List<GPU> gpus = Arrays.asList(new GPU("GTX-1080", 700, 2560, 1607, 1733, 8),
       new GPU("GTX-1070", 550, 1920, 1506, 1683, 8),
       new GPU("GTX-970", 330, 1664, 1050, 1178, 4),
       new GPU("GTX-770", 400, 1536, 1046, 1085, 2));

gpus.sort(new Comparator<GPU>() {
   @Override
   public int compare(GPU o1, GPU o2) {
       return o1.getPrice() - o2.getPrice();
   }
});

for (GPU gpu : gpus) {
   System.out.println(gpu.getName());
}


Output:
GTX-970
GTX-770
GTX-1070
GTX-1080
~~~
{: .language-java}

First I declare a List of GPUs and initialize them with the correct specs. We want the cheapest GPU for a budget gaming build, so the sort function is called and a                  new Comparater is defined. Now the sort function knows that we want to filter the GPUs based on price specifically cheapest to expensive. Looking at the code it took ten lines from the call of the sort function till the results are printed. Ten lines? Come that’s not bad, especially since a lot of that code is generated by just pressing tab. Okay, lets say I want to sort the GPUs based on the other specs provided lets review that code. 

~~~
List<GPU> gpus = Arrays.asList(new GPU("GTX-1080", 700, 2560, 1607, 1733, 8),
       new GPU("GTX-1070", 550, 1920, 1506, 1683, 8),
       new GPU("GTX-970", 330, 1664, 1050, 1178, 4),
       new GPU("GTX-770", 400, 1536, 1046, 1085, 2));

gpus.sort(new Comparator<GPU>() {
   @Override
   public int compare(GPU o1, GPU o2) {
       return o1.getPrice() - o2.getPrice();
   }
});

for (GPU gpu : gpus) {
   System.out.println(gpu.getName());
}
System.out.println("==");
gpus.sort(new Comparator<GPU>() {
   @Override
   public int compare(GPU o1, GPU o2) {
       return o2.getcudaCores() - o1.getcudaCores();
   }
});

for (GPU gpu : gpus) {
   System.out.println(gpu.getName() + " cuda Cores:" + gpu.getcudaCores());
}
System.out.println("==");

gpus.sort(new Comparator<GPU>() {
   @Override
   public int compare(GPU o1, GPU o2) {
       return o2.getCoreClock() - o1.getCoreClock();
   }
});

for (GPU gpu : gpus) {
   System.out.println(gpu.getName() + " Core Clock speed (MHz):" + gpu.getCoreClock());
}
System.out.println("==");

gpus.sort(new Comparator<GPU>() {
   @Override
   public int compare(GPU o1, GPU o2) {
       return o2.getBoostClock() - o1.getBoostClock();
   }
});
for (GPU gpu : gpus) {
   System.out.println(gpu.getName() + " Boost Clock speed (MHz):" + gpu.getBoostClock());
}
System.out.println("==");
gpus.sort(new Comparator<GPU>() {
   @Override
   public int compare(GPU o1, GPU o2) {
       return o2.getVram() - o1.getVram();
   }
});

for (GPU gpu : gpus) {
   System.out.println(gpu.getName() + " VRAM GB" + gpu.getVram());
}
System.out.println("==");

Output:
GTX-970
GTX-770
GTX-1070
GTX-1080
==
GTX-1080 cuda Cores:2560
GTX-1070 cuda Cores:1920
GTX-970 cuda Cores:1664
GTX-770 cuda Cores:1536
==
GTX-1080 Core Clock speed (MHz):1607
GTX-1070 Core Clock speed (MHz):1506
GTX-970 Core Clock speed (MHz):1050
GTX-770 Core Clock speed (MHz):1046
==
GTX-1080 Boost Clock speed (MHz):1733
GTX-1070 Boost Clock speed (MHz):1683
GTX-970 Boost Clock speed (MHz):1178
GTX-770 Boost Clock speed (MHz):1085
==
GTX-1080 VRAM GB8
GTX-1070 VRAM GB8
GTX-970 VRAM GB4
GTX-770 VRAM GB2
~~~
{: .language-java}

This block of code is nothing but doing a copy and paste of our initial sort. You should never copy and paste chunks of code to achieve your goal that’s just lazy, and not the good kinda lazy! The above code is difficult to read, repeats itself, and adds unnecessary length to the code base. 

Sorting with Lambdas:

~~~
List<GPU> gpus = Arrays.asList(new GPU("GTX-1080", 700, 2560, 1607, 1733, 8),
       new GPU("GTX-1070", 550, 1920, 1506, 1683, 8),
       new GPU("GTX-970", 330, 1664, 1050, 1178, 4),
       new GPU("GTX-770", 400, 1536, 1046, 1085, 2));

gpus.sort((g1, g2) -> g1.getPrice() - g2.getPrice());
gpus.forEach((g) -> System.out.println(g.getName() + " price $" + g.getPrice()));
~~~
{: .language-java}

With lambdas I was able to accomplish the same thing with just two lines of code! How does the code look if I sort based on the other specs…

~~~
List<GPU> gpus = Arrays.asList(new GPU("GTX-1080", 700, 2560, 1607, 1733, 8),
       new GPU("GTX-1070", 550, 1920, 1506, 1683, 8),
       new GPU("GTX-970", 330, 1664, 1050, 1178, 4),
       new GPU("GTX-770", 400, 1536, 1046, 1085, 2));

gpus.sort((g1, g2) -> g1.getPrice() - g2.getPrice());
gpus.forEach((g) -> System.out.println(g.getName() + " price $" + g.getPrice()));

gpus.sort((g1, g2) -> g2.getcudaCores() - g1.getcudaCores());
gpus.forEach((g) -> System.out.println(g.getName() + " cuda Cores: " + g.getcudaCores()));

gpus.sort((g1, g2) -> g2.getCoreClock() - g1.getCoreClock());
gpus.forEach((g) -> System.out.println(g.getName() + " Core Clock Speed (Mhz): " + g.getCoreClock()));

gpus.sort((g1, g2) -> g2.getBoostClock() - g1.getBoostClock());
gpus.forEach((g) -> System.out.println(g.getName() + " Boost Clock Speed (Mhz): " + g.getBoostClock()));

gpus.sort((g1, g2) -> g2.getVram() - g1.getVram());
gpus.forEach((g) -> System.out.println(g.getName() + " VRAM (GB) : " + g.getVram()));

~~~
{: .language-java}

It’s so much easier to read, the number of lines are drastically reduced, and your coworkers will thank you for using lambda expressions.

That's It For Today
---
Hopefully you have a high level understanding for what lambda functions are and how they can be used. This post wasn’t designed to be technical overview of the Java 8 lambda expressions, but instead help someone understand what a lambda expression is, how to identify them, and some use cases. At times it’s better to start with a broad overview of a topic before diving deep into it. That’s it for today! 