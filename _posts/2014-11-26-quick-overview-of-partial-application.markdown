---
layout: post
title:  "quick overview of partial application"
date:   2014-11-26 02:34:19
categories: partial-application javascript
---

One of the thing that I like most about JavaScript is that it can also be written in a functional way and partial application is one of them. 

##What is Partial Application?

If you look at the name itself then it should give a *partial* hint on what a partial application is. Partial Application is a way of partially filling a function with arguments early on and be able to use that information in the later part of your application. It is mostly possible because of closure.  
To have a partial function we first need a wrapper function which takes, either left or right argument as a function to apply the partial application to. The arguments provided early on are held for the entirety of application due to the help of closure while a complete new function is returned with the bounded function (also passed in as a parameter) being invoked with the partial(arguments passed early on) and later arguments.

*Note that there are different variation of partial applications. Left, right and full application. But we'll just talk about left partial application here as that's the most commonly used and easy to understand.*

##Usage:

Partial Application is pretty powerful if you know how to use it. I mostly use it when I have a repeated task which produces different output based on the provided input (parameters). And because functions are first class citizens in JavaScript, it becomes even more powerful as you can inject your custom function to the already predefined one. 

Let's see this in code:-

Let's say we want to have a function that adds 1 to all the arrays on the given argument.

    var addArray = function(arr){
		arr.forEach(function(i){
			console.log(++i);
		})
    }

And you realize that after some times you want to do more with arrays, let's say mulitply them by 2.

	var multiplyArray = function(arr){
		arr.forEach(function(i){
			console.log(i*2);
		})
	};

Now instead of making a seperate function and iterating over the array over and over, you can create a generic partial function to iterate over the array and then just perform your task by giving the callback to the partial function. Let's see that in action.

	//First we create a partial function generator
    var partial = function(fn){
		var self = this,
			arg = Array.prototype.slice.call(arguments, 1);
		return function(){
			var allArgs = args.concat(Array.prototype.slice.call(arguments));
			fn.apply(self, allArgs);
		};
    };

If you fairly know about JavaScript then it's not that hard to get your head around what's happening on the above code. Probably just a pattern that you haven't thought about before. But let's see what's happening anyway.

We start with regular `function expression` with one parameter, this is the parameter that accepts function to be bounded or partialized (not the official terminology).  
Inside the function we hold all the arguments other than the `fn` or the first one (*by hold I mean it will be remembered even after the function returns because we're returning a function*). These are the arguments that you want to be prefilled for further usage in your application.    
On the return statment we're accepting more parameters to be filled on the latter part of our application and we finally invoke the *partialized* function with all the parameters (with prefilled and latter filled)

	//Now we make our repetitive function into a partial function
	//But we have to modify our function a little
	var loopArray = function(arr){
		arr.forEach(Array.prototype.slice.call(arguments, 1)[0]);
	};
	
	//Now let's make a partial function
	var partialized = partial(loopArray, [1,2,3,4]);
	//We can now use partialized function to accept any callbacks
	//on the prefilled iteration

	//Our initial adder function
	var addArray = function(arr){
		console.log(++arg);
	}
	
	//Using the partial function
	partialized(addArray);

Let's slow down and explain what I'm doing. The `loopArray` function is made in such a way that the second parameter is treated as a callback for our `forEach`. You could also do :-

	var loopArray = function(arr, callback){
		arr.forEach(callback);
	};

But it's totally upto you. The `arguments` way can be a little vague but is more dynamic, meaning you have to do little to change in your code to change the behavior of your function while the explicit declaration of parameter is more rigid but more clear.  
Anyway, the point here is we're in agreement that the second argument will be a callback. On `partialized function` expression we specify the array that we want to be prefilled. This parameter is going to be used with all the function that uses this partial application.  
Now, we just create callbacks and pass it to the partial application as we're doing with `partialized(addArray)` `addArray` here is our callback function.

Although this is not the greatest example of partial application, I just wanted to show a bit practical example other than just adding 1 to a number like you see in other posts.

You can do the same for multiply too. You can play more on the jsbin below.

<a class="jsbin-embed" href="http://jsbin.com/dihobi/1/embed?js,console">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

You can read more about right, full partial and other cool stuff on the links below:-

[http://benalman.com/news/2012/09/partial-application-in-javascript/](http://benalman.com/news/2012/09/partial-application-in-javascript/)  
[http://ejohn.org/blog/partial-functions-in-javascript/](http://ejohn.org/blog/partial-functions-in-javascript/)
