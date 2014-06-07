---
layout: post
title:  "Ranges in npm."
date:   2014-06-07 02:34:19
categories: node npm
---

I'll keep this post really short as I have been quite busy and will still be for the next two weeks. 

So, this week in one of the JS meetup [Jed Mao](https://twitter.com/mrjedmao) brought up a really good point on why he uses `npm` from the `cli` instead of writing the package name in `package.json` and just doing `npm install`. 

If you want to make any sense of the paragraph below then make sure that you know what [`semver`](http://semver.org/) is. 

Before I put forth his reasoning, I was all up for just writing our `deps` in `package.json` and just doing `npm install` from the cli. But there are something called `ranges` that you can specify in the version number for your dependencies so that only those dependencies that are in the range gets downloaded. The comprehensive list of ranges is posted at the link below but in summary here are the ranges that you can specify :-

`>1.0.0` :-  
greater than a specific version number. You can also use `<`, `>=`, `<=`  
`>1.0.0  <2.0.0` || `1.0.0 - 2.0.0` :-  
whatever the version between v1 and v2. You can use the first or the second one. Again, you can use `<=`, `>=` likewise.  
`~1.0.0` :-  
accepts all patches, meaning `>=1.0.1 - <1.1.0`  
`^1.0.0` :-  
accepts all pre-releases (minor release), meaning `>=1.0.0 - <2.0.0`  
`1.0.x` || `1.0.*` || `1.0` :-  
accepts all the version that matches `1.0` versions  
`*` || `x` || `""` :-  
any version whatsoever (usually the recent one)  

The last one on the list was my proposal to include in the `package.json` file so that one doesn't have to go lookup the version number for each `dep`. But like Jed pointed out it can have bad consequences and I was naive to even think `*` was an option. `*` means all the major, minor and patches. Major changes could mean that it will break your application if you rely on version specific features. 

So, Jed's advice made perfect sense to me. His advice was to just use `cli` like so :-

`npm install express mongodb mocha sinon grunt` 

Doing the above will install the latest stable build and npm will also put the actual version name with `^` at the prefix. Again, `^` will install all the pre-releases and should not break your application.

To keep track of your versions you can also use [Gemnasium](https://gemnasium.com/) as mentioned by Jed in the meetup. 

[More about ranges from Issac.](https://github.com/isaacs/node-semver#ranges)