---
layout: post
title:  "when it's apt to use global"
date:   2014-07-14 01:34:19
categories: node chaijs mocha	
comments: true
---

Pretty much almost any programmer that you meet would say that sticking variables to `global` object is a bad idea. JavaScript was pretty notorious for this one. Forget to put `var` in your variable inside the `function` and it goes straight to the `window`(global) object if you forgot `'use strict';` in your function or in global context. It's still true for node but instead of `window` object it gets added to `global` object.  
`Global` is well, global object in node, just like `window` in browsers. You can put your variables in global object by either not declaring it with `var` or using `global` or `GLOBAL`. *`GLOBAL` is just an alias for global.* 

So, let's just demonstrate the above with some code. Create two files with the following content and then run `node index`;

<script src="https://gist.github.com/sinkingshriek/7ac40170e5d2606b4180.js"></script>

As you can see from the above code that we didn't really export `someVal` from globalMe and yet the `index.js` was still able to find it. And as you may have already guessed it, it's because that we're sticking this into `global` object.

Okay, so enough of the demonstration and to get to the point on the real topic at hand. Why would we ever want to use it if it's a bad practice?  
There are times when you just can't avoid using `global` object. Just recently I *had* to use `global` object to get my code running which brings me to another topic.  

##How to use chaijs in your mocha.opts ?

I won't go into details on what `chai` or `mocha` is in this post. And if you've been playing around with mocha then you know you can have `mocha.opts` file in your `test` folder which is basically options for your `mocha` command. So instead of doing:-

    mocha --required should --reporter nyan

You can do :-

    //mocha.opts
    --required should
    --reporter nyan

But since `chaijs` is a bundle of three different assertion libraries you can't do `mocha --required chai().should`. That's just absurd. Your command line doesn't invoke functions. So how do you go about achieving `chai` with `mocha`? Globals!!

So if you create a `node_module` with some file called..let's say `chai-mocha-wrap.js` with the following globals :-

<script src="https://gist.github.com/sinkingshriek/40e6cd6728e0c39e883d.js"></script>

Then you should be able to do :-

    //mocha.opts
    --required chai-mocha

And have chai assertion on all your mocha tests. You also have the option to use `should`, `expect` or `assert` (yay freedom!!).

Although, this is an ideal case for the usage of `global` you should always, **always** document why you're using `global`.

