---
layout: post
title:  "How node modules actually load."
date:   2014-05-30 02:34:19
categories: node
---

*Edit:- This post has been modified as of 2014-12-06 to correct and clarify few issues in this post*

I'll take a break from the devtools post and post a quick info that I've been reading up this week.  
Before jumping to the topic let's first look at what module really is.

In general, a module is nothing but a separated functionality (piece) of a program that gives structure to your code while adhering to two key features, **low** `coupling` and **high** `cohesion`, meaning that the piece of code that you write has very less or no dependency (**low coupling**) and can be pluggable to almost any part of your other program (**high cohesion**). Basically, these two terms go hand in hand and are co-related.


So, think of node modules as just a JavaScript module and you plug these modules in your program like this:-  

`var myModule = require('./myModule');'

This is what the meat of this post is going to be. What exactly is happening when you do `require`? 

##Requiring as file
Before I go any further notice that `./` before the module name in that require? Whenever you supply `./` to `require` then node will search for the module in the current working directory or also called `relative path`. So if you had your file structure as follows:-

    --myRootDirectory
    ----index.js //let's say this is where your require('./myModule') is
    ----myModule.js

Then doing `./` will immediately search inside the current directory. If you would feel like putting your modules in a directory then that's perfectly fine too. Just add `./directory_name/myAnotherModule`.

##Requiring core modules
Node comes with some core modules. If you simply `require('fs')` (fs is file system core module), then node knows that it's a core module and simply requires that.

##Requiring previously installed modules
There are thousands of packages in `npm` that you can require. If you install some package in your project then you'll notice a directory show up called `node_modules`. To require these modules you would simple require the module name like:-  

    var _ = require('lodash');

When you do this, require will try to look for `node_modules` directory in current directory. If it does find it and the required package is installed in this directory then it simply loads from there but if it doesn't, then it goes to the parent directory and looks for `node_modules` there and so on and so forth until it reaches the root of the path. This is why you have to be careful where you install your package. Sometimes it can get confusing if you simply require a package and even though it's not in your current `node_modules` it simply works and in future that gets deleted and you have no idea why your app suddenly stopped working.

##Requiring as directory
As similar to requiring as file, you can also require as a directory by simply doing:-  

    var _ = require('./');

If you do so, then node will try to look for `package.json` manifest file in the current directory and looks for `main` property. `main` property is the property in the `package.json` file where we specify our entry file. So let's say if you had an app then you could just do `npm start` and then it reads the `package.json`file for `main` property and executes that script. But let's move as this is a good topic for next post.  
If however you don't have `package.json` file in the directory that's specified in the require then it just automatically assumes that you have `index.js` on that directory. **Again, same thing for npm start too**

Also, do note that you don't have to specify `.js` or `.node`. Node looks for `.js` or `.node` automatically.

