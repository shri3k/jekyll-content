---
layout: post
title:  "revisiting new"
date:   2014-09-06 01:34:19
categories: javascript intro 	
comments: true
---	
It's almost embarrassing at times when I go complete blank on how `new` actually works. We don't think much when creating instances in JavaScript. It just works. And that's also probably why it's so easy to forget what's happening under the hood of this three lettered word.

Basically, there are three things that's happening under this three lettered word.

- [`__proto__` being referenced](#__proto__-being-referenced)
- [`constructor` being referenced](#constructor-being-referenced)
- [Constructor being called with..](#Constructor-being-called-with)

###`__proto__` being referenced:-
Whenever we create an instance we're always trying to create an instance from some constructor (*Constructor is just some normal JavaScript function*). The first thing that can happen whenever we're creating an instance is that the `__proto__` of an instance being referenced to the prototype of the Constructor (*Remember, `__proto__`, which is also called prototype, is different from Constructor's prototype. `__proto__` is a hidden property of pretty much every object in JavaScript*). Let's see this in action.  

Let's mimic the behavior of `new` without using `new`.

{% highlight js%}
function SomeConstructor(name){
	this.name = name || "hi";
}
var someInstance = {};
{% endhighlight %}

Here, we just start off with a Constructor and an empty object literal.

{% highlight js%}
function SomeConstructor(name){
	this.name = name || "hi";
}
var someInstance = {};
someInstance.__proto__ = SomeConstructor.prototype;
{% endhighlight %}

And then, `__proto__` is referenced to prototype of Constructor.

###constructor being referenced:-
Now that the `__proto__` is being referenced to the Constructor's prototype, we now have to reference the instance's constructor to the Constructor (*function*). (*Every instance has a constructor property referencing to it's Constructor*)

{% highlight js %}
function SomeConstructor(name){
	this.name = name || "hi";
}
var someInstance = {};
someInstance.__proto__ = SomeConstructor.prototype;
someInstance.constructor = SomeConstructor;
{% endhighlight %}

###Constructor being called with:-
This IMO, is the crucial part among all three steps. This is where all the instance variables and methods gets assigned to the new instance. `new` finally calls the Constructor function with reference to our instance. 

{% highlight js %}
function SomeConstructor(name){
	this.name = name || "hi";
}
var someInstance = {};
someInstance.__proto__ = SomeConstructor.prototype;
someInstance.constructor = SomeConstructor;
SomeConstructor.call(someInstance); //SomeConstructor.apply(someInstance) is valid too
{% endhighlight %}

And that's how new works. This is an important step to learn how Inheritance works in JavaScript.

Do note that `this` inside the Constructor function is what's telling the variables to be instance variables and functions to be methods, meaning the new instance will have a copy of these variables and functions to itself. We can also assign our functions and properties to the prototype of Constructor to make it share among instances but that's a whole different topic for another post.





