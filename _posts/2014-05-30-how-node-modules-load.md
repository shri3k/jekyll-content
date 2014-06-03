---
layout: post
title:  "How node modules actually load."
date:   2014-05-30 02:34:19
categories: node
---

I'll take a break from the devtools post and post a quick info that I've been reading up this week.  
Before jumping to the topic let's first look at what module really is.

In general, a module is nothing but a separated functionality (piece) of a program that gives structure to your code while adhering to two key features, **low** `coupling` and **high** `cohesion`, meaning that the piece of code that you write has very less or no dependency (**low coupling**) and can be pluggable to almost any part of your other program (**high cohesion**). Basically, these two terms go hand in hand and are co-related.


So, think of node modules as just a JavaScript module and you plug these modules in your program like this:-  

`var myModule = require('./myModule');'

This is what the meat of this post is going to be. What exactly is happening when you do `require`? Before I go any further notice that `./` before the module name in that require? Whenever you supply `./` to `require` then node will search for the module in the current working directory or also called `relative path`. So if you had your file structure as follows:-

    --myRootDirectory
    ----index.js //let's say this is where your require('./myModule') is
    ----myModule.js

Then doing `./` will immediately search inside the current directory. If you would feel like putting your modules in a directory then that's perfectly fine too. Just add `./directory_name/myAnotherModule`.

On a similar note, you can also do:-

`var myModule = require('/myModule');` //note the absence of `.`

for `absolute path`. This looks at the root of your app instead of the current directory. So if you had:-

    --myRootDirectory
    ----myAnotherDirectory
    ------myModule.js //let's say this is where your require('/myAnotherModule') is
    ----index.js 
    ----myAnotherModule.js

Also, do note that since node already expects a JavaScript file you don't have to put the `.js` extension at th end.

So that was with the `./` and '/' at the prefix of your module but there's another mechanism node follows to look for your modules. If you don't supply the `./` at the prefix then the following steps is followed by node to search for your module.

* Checks if it's a core module.
* Checks the module in `node_modules` directory in the current directory and moves up one directory until the root of your app.
* Lastly checks in the path of `NODE_MODULE` that's specified in your system's `environment variable`.
* At the end throws exception if nothing is found.

The core module that node checks in the beginning are the core modules that node provides like `http`, `fs` which will take precedence over any other steps.  
If it's not a core module then it starts looking for `node_modules` until it reaches the root of your application. Something like this:-

    --myRootDirectory
    --node_modules
    ----myModule.js
    --lib
    ----myAnotherModule.js //you can require('myModule') here.
    --index.js // and here require('myModule') will reside.

See how `lib` directory doesn't have `node_modules` and it goes up one level in `myRootDirectory` where it does have `node_modules` directory which indeed has `myModule`.  
Also, in the index.js, which is inside `myRootDirectory` has `node_modules` directory so it just immediately knows where `myModule` is.  
NOTE:- node doesn't traverse down so if you had something like this:-

    --myRootDirectory
    --node_modules
    ----myModule.js
    --lib
    ----node_modules
    ------myInsideModule.js
    ----myAnotherModule.js //you can require('myInsideModule') here.
    --index.js // but not here. THIS WILL NOT WORK require('myInsideModule')

However, if your module is inside your system path (`ENVIRONMENT VARIABLE`) then your module should load just fine. For Windows users it's usually in 
`c:\Program Files\Node\lib` and for Linux users just `echo PATH` and google on how to add path in your `environment variable`. 

You can play with the [sample node_modules](https://github.com/sinkingshriek/load_node_module) that I've made ready for you play. Check out the commits too on what mistakes I made to learn from it.  


