---
layout: post
title:  "event loop and related"
date:   2014-10-20 02:34:19
categories: javascript browser v8 node
---

So, recently I watched the talk from [Philip Roberts](https://www.youtube.com/watch?v=8aGhZQkoFbQ). If you have time then I highly recommed that you watch that talk instead of reading this because I am just going to summarize from that talk. You can think of this as TL;DW version of that talk.

Okay, let's just dive in and see how JavaScript actually works and how this event loop and others play part (I'm going to assume that you already know that JS is single threaded and concurrent language). 

Probably the most crucial part for me that clicked to understand how JavaScript really works the way it does was that Web Browsers or NodeJS consists of many components. Those components are V8, SpiderMonkey, Chakra, Crackan or any other JavaScript engine, APIs (this is your DOM, ajax, filesystem and other APIs that Web Browsers or NodeJS provide you, you can find more [Web API here](https://developer.mozilla.org/en-US/docs/Web/API) and [Node API here](http://nodejs.org/api/)), a callback queue and finally an event loop. There might be more but these are pretty much the basics. 

For simplicity's sake I'll just talk about browsers but it applies to server-side part of JavaScript too.  
Okay, so browsers consists of :-

  - JS Engine
  - Web APIs
  - Callback Queue
  - Event Loop

JS Engines like V8 and SpiderMonkey in turn consists of :-

  - Heap
  - Stack *(This is the same stack from the callstack that you see in the browser)*

I'll explain this by running the following code. You can run it in your browser and see the output too.

<script src="https://gist.github.com/sinkingshriek/c1d08d566ee535d8f8b2.js"></script>

Let's examine the code.  
We start off with some variable declaration and print `First`. Currently you're on some level of stack. Stacks are what's being currently run or processed by the JS Engine.  

And we have the `increase` function which is recursive. It takes upto 10000 stack memory. The code doesn't have to be a recursive function, any JavaScript code executes on some stack. Only what's at the top of the stack tells us what's currently running. You can put a debugger inside `increase` function to see the stack increase as it calls itself over and over. The JS being a single threaded doesn't run anything else apart from this code until it finishes. What does this tell us? This tells us that this part is still sync or blocking code.    

So the `Third` doesn't get printed until the `increase` loop completes. For shits and giggles, you can try to exceed the `10000` to some ridiculous number. I believe 160000 is the maximum that Chrome allows at the moment. And if it exceeds none of the following code gets executed.  

Anyway as soon as it reaches to `setTimeout` function however, it pushes that callback function to Web API and let's it handle what the code is doing there. Mind you that this hasn't yet been pushed to stack. It waits for 5 secs until it can execute.  
So, `Third` gets printed out and we have another `setTimeout`, well, this gets pushed to Web API as well but since it's 0 sec you might think that it should get exectued immediately. Nope. After Web API is done with the code, it gets thrown in `Callback Queue` and it waits its turn for `event loop` to put it on the stack. The `event loop` doesn't put the `callback` on the stack until the stack has been cleared.   

So it proceeds to print `Fifth` and finally the stack is free and `event loop` says "okay, you're allowed to pass to the `call stack` now". And then it prints `Fourth`.  

Meanwhile the `setTimeout` with 5 sec delay, after 5 sec, gets pushed into the `callback queue` and then `event loop` pushes it to stack and `Second` is the last one to get printed.  

###A lot to process?
Think of JavaScript as your Post man and think of all the events as all the letters that Post Man has to deliver. Well, your Post Man doesn't really read all your letters. He just needs to now where to put it like Web API or database query which is handled by other processes. The letters could be something that you need to read...and if it's a bill and you need to mail back the check then you need to write down the check and then mail them from the Post Office. All this work is taken care by you (let's call you a Web API or ajax or any database query) and Post Man, aka js, just goes and does his business.

Back to our Post Office. Let's say your post box delivers mails as soon as it's being handed to them. And you hand him the mail that has check that you just made out to your electricity provider. This futuristic post office is like `callback queue` in browsers. And then it gets deilvered which one came to the Post Office first. And the process continues.

Now let's say the postman is stuck in the traffic. This can be thought of as blocking code in js. `increase` is such type of code which is why many people often discourage writing memory intensive code in JavaScript but really, what they're saying is not to write blocking codes if you can avoid it. 

There are very good reasons to write sync/blocking code and there are times when it's just not needed. But anyway, this requires a blog post of it's own. I hope this somewhat clarifies how JavaScript works the way it does. 
