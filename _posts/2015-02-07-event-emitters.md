---
layout: post
title:  "EventEmitter"
date:   2015-02-07 01:34:19
categories: node basics event-emitter
---

Before even describing what Event Emitter is, I think it's important to note that Event Emitter is one of the fundamental concept in, not only node, but in JavaScript in general. In layman terms, Event Emitter is pretty much(well, partly) `addEventListener` in client-side JavaScript where we attach events like `click`, `keydown`, `load`, etc but with higher abstraction.  
It is also very much used to decouple the code and avoid the callback hell. If you're still not convinced enough then feel free to google. It's also inherited by the `stream` module which merits a seperate post in itself.  

##What is EventEmitter?
I've already hinted what EventEmitter is above, but let's go a little deeper. As you know already that the `WebAPI` gives you few predefined events (click, load, keydown, etc) by default. With EventEmitter, you can make your own events. The events can have any names as you please (but make it meaningful). If I wanted to have an event called 'callMeOnlyWhenNeededPlease', you absolutely can. The syntax is the same as you're used to from the client-side JavaScript. So what's the difference between the browser `attachEventListener` and this one? Apart from being able to create your own `events'` not much. Browser level JavaScript just hides the implementation but EventEmitter exposes that for you. That's the main difference. However, you can absolutely simulate the EventEmitter module in browser by either using jQuery's `trigger` method or DOM API which is [explained here](http://www.2ality.com/2013/06/triggering-events.html). Or if you want to do it in plain JavaScript then goto the [Deep Dive](##Deep_Dive) section.
Okay, let's look at an example on how it exactly works to get the feel of syntax.

Here are all the [methods and properties](http://nodejs.org/api/events.html) that EventEmitter exposes.

    var EventEmitter = require('events').EventEmitter;
    var eventEmitter = new EventEmitter();

	  // `on` is just an alias for addEventListener
    // you could have easily done eventEmitter.addEventListener too
    eventEmitter.on('callMeOnlyWhenNeeded', function(){
         console.log('I've been summoned);
    });
    eventEmitter.emit('callMeOnlyWhenNeeded');

This should look familiar to you if you already use 'addEventListener'. In fact, it's the same. You have your event name on your first argument and then the callback or function to be invoked(listener) when that event is invoked.  
We invoke or call events using `emit` method from `EventEmitter`. Really straight forward. As mentioned above, there are more than `on` or `addEventListener` methods in EventEmitter. Most frequently used are `once` and `removeListener`. The latter is pretty self-explainatory and the prior is called only once and is removed after being invoked.  
Protip:- It's good practice to have `error` event attached on all your EventEmitter, if you don't and someone tries to call event called `error` then it just throws `Uncaught, unspecified "error" event.`.  

##Deep Dive
Let's now try to see what exactly is happening behind the scene. You're free to look at the source code of node too if that's what you prefer but I'll try present it in much simpler fashion.

<a class="jsbin-embed" href="http://jsbin.com/fahovu/1/embed?js,console">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

Surprised on how simple the implementation is? Well, because it is and also I trimmed the code a lot to just give the core concept. There's definitely no error handling, arguments handling and many methods are not included on above code but that's the basic concept. This clever but simple pattern can make your code really easy to read and have much more control over your program. 

*Although, io.js is out already I have referenced node's official API. Nothing much has changed in io.js in this field but feel free to make a comparison [there](https://iojs.org/api/events.html). *
