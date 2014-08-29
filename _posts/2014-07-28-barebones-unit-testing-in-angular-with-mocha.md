---
layout: post
title:  "barebone unit testing in angular with mocha"
date:   2014-07-28 01:34:19
categories: node angular chaijs mocha	
comments: true
---
*tl;dr:- wanted to do testing in angular, found tuts that showed dep with karma, writing without one*  
I've been trying to get my hands dirty on angular and while the tutorial in the official angular site showed some testing code, it never showed how to set one up. So, like any person I started googling for a quick tutorial on setting up unit testing for angular but every tutorial that I found wanted me to install `karma` first. I've played with `karma` before and didn't really see the point of needing `karma` for quick unit testing. As [Vojta Jina](https://twitter.com/vojtajina "Creator of Karma") himself says, `karma` is a test-runner, that's it. It's not required for your unit tests at all. It's just something nice to have to automate the process. Below are the following things that you do indeed need:-

- mochajs
- chaijs

These are the only two dependencies that you will be needing in your test code. *Of course if you're using `jasmine` you don't need `chaijs` either. But since I like freedom and `chaijs` allows me to use any assertion library I'm gonna go ahead and use that with `mochajs` which is just a testing `wrapper` without any `assertion` (that's why chai is for).

You can download `mocha` and `chai` with npm but for simplicity's sake I'm gonna go ahead and just point those to cdn.  

##Setup
You would need:-

- index.html
  - [mochajs](#mocha)
  - [chaijs](#chai)
  - [angularjs](#angular)
  - [div with `mocha` id in it](#mocha)
  - [testing style setup](#testing-style-with-optional-assertions)
  - [your_app_code.js](#code-to-be-tested)
  - [your_test_code.js](#test-code)
  - [run mocha](#running-the-tests)

The order for:-

  - angularjs
  - mochajs
  - chaijs

doesn't really matter but `mocha setup` has to come after `mochajs`. The order for `your_app_code.js`, `your_test_code.js` and `run mocha` **absolutely matters**. Get that ordering wrong and you'll spend almost 15 mins trying to debug what exactly is happening. 

Okay, let's get started with the code.

##Code

###mocha
Since we're doing this without `karma` you would need a plain `html5` file. We're gonna go ahead and add `mochajs` in the page with `mocha` css and `div` that `mocha` requires which is in `line 10`  

<a class="jsbin-embed" href="http://jsbin.com/fiqoze/1/embed?html">MySite</a><script src="http://static.jsbin.com/js/embed.js"></script>

###chai
Now, let's go ahead and add `chaijs` too. 

<a class="jsbin-embed" href="http://jsbin.com/fiqoze/2/embed?html">MySite</a><script src="http://static.jsbin.com/js/embed.js"></script>

###testing style with optional assertions
I like to setup my `mocha setup` after I include `mocha` and `chai` because I'm adding `var assert = chai.assert` which requires `chaijs`. You could have added `var assert = chai.assert` in your test code too but I like to add them in global. It makes sense to put them in global because istead of having them to write that over and over on each of our test files you just write once and if you do need some other assertion in your file you can also include them in your individual files.

<a class="jsbin-embed" href="http://jsbin.com/fiqoze/3/embed?html">MySite</a><script src="http://static.jsbin.com/js/embed.js"></script>

###angular
We now include `angular` (You can change the version here. At the moment of writing this, this was the latest stable version).

<a class="jsbin-embed" href="http://jsbin.com/fiqoze/4/embed?html">MySite</a><script src="http://static.jsbin.com/js/embed.js"></script>

###code to be tested
Now, include the code that you want to test. This includes all your `controllers`,`services` everything. Here, let's assume that I have them all in my `app.js` file. 

<a class="jsbin-embed" href="http://jsbin.com/fiqoze/5/embed?html">MySite</a><script src="http://static.jsbin.com/js/embed.js"></script>

###test code
We finally include our `spec` file or `test` file on our page (Usually, I put them under test folder which is like the standard convention now but you can put wherever you want depending on your preference).

<a class="jsbin-embed" href="http://jsbin.com/fiqoze/6/embed?html">MySite</a><script src="http://static.jsbin.com/js/embed.js"></script>

###running the tests
We're not done yet. We have to now tell `mocha` to run our `tests`.

<a class="jsbin-embed" href="http://jsbin.com/fiqoze/7/embed?html">MySite</a><script src="http://static.jsbin.com/js/embed.js"></script>

Open this in Firefox or Chrome(if Chrome's giving you problem with some security issues then switch to Firefox or run it with some simple server). And see your tests passing..or failing. 

If you use `Sublime Text` then you can also use the following snippet I made.

<script src="https://gist.github.com/sinkingshriek/fa546d0e6d2fe06facb5.js"></script>

