---
layout: post
title:  "Imagine trying to build a skyscraper with just a wrench."
date:   2014-05-24 01:34:19
categories: casual devtools
---

Think of us, devs, as construction workers. Let's just pretend for now that we're construction workers. You're given all these ideas and blueprints by architects and business folks and everything that's needed for construction. You are provided with all the possible resources that you can possibly think of to get this herculean task done but the problem is that you have no clue how to operate those big cranes, excavators, forklifts etc but you're an expert in using wrench. I'm not saying it's completely useless, well maybe to screw things when one of the nuts come lose in those bigger machinery but the point is you need to learn tools if you're even conceiving of making big web-apps. This doesn't just apply to web-apps. This applies to pretty much in all fields. So you, as a web-developer better know your tools well.

_I'm going to split this post into multiple posts as it got much bigger than I expected._

##DevTools
There are plenty of tutorials, tips and tricks on dev tools in the Internet and this one isn't going to be different either. I can't stress this enough. Learn to master *DEVTOOLS*. It might feel like people are trying to shove it down your throat and they're right to do so as it's a crucial part of being a web-developer. If you're going to be a decent developer then not knowing this should be a crime. This post is not going to dive into details into all the possibilities that devtools has to offer but give you overview on what power it holds. However I will present the link to go deeper on them if you want to know more of the APIs. I will try to cover for both Firefox and Chrome (sorry IE) but not going to do one on one comparison. Oh and another disclaimer is that this is geared more towards PC so if you're Mac user then the shortcut might be a little different. Usually, the `Ctrl` is replaced with `Cmd`. Okay, let's begin.

##Opening devtools
The fastest way to get to the developer tools in either Chrome or Firefox is by pressing `Ctrl + Shift + I`. Others might prefer `F12` but I usually go for the former since `F12` is much farther than those three keys. You can also take the old route of right clicking and selecting `Inspect Element` which will open up your devtools with `Element panel` being active and focusing on the element that was pointed to by your mouse (assuming that the mouse click is on your browser). There are also other ways to open your devtools like going to the Settings -> Tools -> Developer Tools in Chrome or Tools -> Wev Developer -> Toggle Tools in Firefox. 

##Changing Settings of devtools
Shortcut:-
_In Chrome only_
`?` or `F1`   
You have your devtools open and before you do anything I highly, highly recommend you change some of the settings first. I've seen many people never enter settings even after their heavy usage of devtools. There are some really powerful options available under that settings. All you have to do is press that `gear icon` on your devtools in Chrome and in Firefox or if you're in Chrome then you can just use the shortcut provided above. The options might not be at the exact place in Firefox and Chrome but if you read through some of them then it's not that hard to make connections between them. Also, not all options of one browser is guaranteed to be available in another, so depending on what you want you might favor one over another. But that's a different topic on it's own let's just change some settings for now:-  

* Disable Cache _//this way your browser doesn't cache your js and css so that you can changes promptly after changes._  
_The following pertains only to Chrome DevTools_
* Enable Ctrl + 1 - 9 shortcut to switch panels (self-explanatory which lets you jump to the tabs by pressing `Ctrl + [numeric key]`)
* [Workspaces tab](#workspace_tab) _//if you want to live code on Chrome which is discussed later_
* [Shortcuts](#shortcuts) _//Learn those shortcuts for maximum efficiency, also discussed later_  
Got your settings all setup? Alright, let's start digging those hidden treasures that devtools has to offer.

##Console Tab
*Shortcuts:-*  
_In Firefox_  

* `Ctrl + Shift + K` _//this will take you straight to the Web Console even if you're in devtools (You'll soon know why I mentioned this)_  
* `Ctrl + Shift + J` _//this will only bring up the Web console window and nothing else._  

_In Chrome_  

* `Ctrl + Shift + J` _//to jump straight to Web Console from your normal browser. Invoking this shortcut again in the devtools will open another devtools for your devtools. This will go as deep as your mind will let you._  
* `Ctrl + 9` _//if you're already in devtools and has enabled `Ctrl 1-9 shortcut to switch panels` as mentioned above._

_____  
If you're a JavaScript developer then you will be spending majority of your time here. So let's start from this tab. The console acts like an interpreter and display all the logging informations. You can pretty much do your full programming here too (although not recommended). Go ahead and start writing your JavaScript expressions and start executing it (`Enter to execute`) and (`Shift + Enter`) to enter multi-line. Before we go full on on what this console has to offer take your time to right-click on the console and see what options it gives you.

_In Firefox_  
Right click will give you option to `log requests and response bodies`. What this means is it 

_In Chrome_  
Right click will give you more options than Firefox although you're not losing any functionality per se.

* Preserve the logs upon navigation  
  This does exactly what it says, if you're jumping over pages then instead of clearing the console on every new page it stores your earlier logs for you.
* Log XMLHttpRequest  
  This is to log your AJAX requests just in case if you ever needed that even though it's available in `Network tab`.

Okay okay, no more transients. Diving in console now.  

###Clear console
_Shortcut_
`Ctrl + L`  
_Alternatives_

* clear();
* Right click -> Clear console _In chrome_
* Clicking the clear icon in the devtools

But the easiest way to clear the console is to just press `Ctrl + L`. That's it. You don't have to type `clear()` or right click and click `Clear Console` or find any obscure icons.

###Last Evaluated expression `$_1`
Before you go there and start trying this thing out in your console make sure what expression is. Basically, expressions are any valid code that resolves to a value. According to MDN, there are two types of expressions. Those that assign a value to a variable and those that are simple value.  
So this:-  
`x = 1` would be first type expression and `1+1` would be second type expression. More on this [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators).  

So, doing `$_` should be pretty clear now that you understand what expression is. Again, `$_` will return the last evaluated expression in the console. To be honest, I don't use this as much. Pressing `up` arrow is far more easier than typing `$_`. But, just in case you need this then you know there's a way to do it.

###Last five DOM elements `$0 - $4`  
Say, you just inspected (right-click on website and select Inspect) something on your website and you want to grab that element. You could do `document.querySelector('some-class-or-id-or-tag')` but why waste time when you can just do `$0`. Do that again and the item that was stored in `$0` earlier will move to `$1` and so on till `$4`. It's not necessary that you have to inspect the element every-time from the website. You can simply select it on `Element Panel` if you already know where that element is and then use it on console with `$0`.

###Query Selectors
* `$(selector)`
* `$$(selector)`
* `$x(path) `

####$(selector)  
So, you know how to get the DOM elements using `$0 - $4` but let's say you have to find out the elements with certain selector. You could inspect all the elements in the site or even search (`Ctrl+F`) in `Element panel` but that's tedious. But wait, you know some JavaScript. So you can just do :-

* `document.getElementById('some-selector');` || 
* `document.querySelector('some-selector');` _//heads up this is not same as `document.getElementById()`_

Or, you could just use the alias of `document.querySelector('some-selector')` in devtools which is `$('some-selector')`. That's it.

####$$(selector)
You try doing `$(selector)` but all you're getting is just one result. And then you realize that it's just doing what `document.querySelector()` was doing and sighed at yourself. Now you need something aliased to `document.querySelectorAll()`. And as you can already figure out from the heading that you can achieve just that by using `$$(selector)` and it will list out an array of all the elements with that selector.

####$x(path)
And if you happen to know [XPath](https://developer.mozilla.org/en-US/docs/Web/XPath) then there's this `$x(path)` for you too.

###Events
* getEventListeners(object)
* monitorEvents(object, [events])
* unmonitorEvents( object, [events])

####getEventListeners(object)  
Okay, so you're quite happy with what you have right now with all the query selectors at your disposal. And soon you realize that it's about time to put some interactivity to your webpage. The most common thing to put interactivity in the webpage is to listen to any events in the page and act accordingly. Sure, if it's a simple site, like this is, there may not be too much event in the page but as your webpage start getting more complex it'll be crucial to know what event is called on a specific element. Again, you could do it manually and go to your editor and start searching for it or you could use tools that devtools provide you and be more productive. `getEventListenters(object)` is just the function that helps you pinpoint what function is being invoked on the specified event. Let's look at an example on how it works.
Syntax:-
`getEventListeners(DOM object,[events]);`  

`getEventListeners(document.querySelector('testme'), "click");`  
What the above statement does is, it lists out the click event for the `testme` element. Of course, you could use `$('testme')` or `$0` to get that element but I'll leave it up to you to be creative with what you can do.

The above returns a click object for that element and the function that will be invoked in the `listener` key.  
Remember, the event parameter is totally optional. If you don't specify it then it will return all the event that's attached to that element.  
Do note however, that if you're adding listenter with `jquery` then the `listener` will display `jquery's` wrapper function than yours. This deserves a post on it's own so I'm not going to dwell more on it but if you want to list the listener that's been done purely by `addEventListener` then `getEventListeners` should be more than enough. If you want to play more then here's the [fiddle](http://jsfiddle.net/shriek/QXC5B/) for you. (Note that since jsfiddle uses iframe you might not be able to find the element)

####monitorEvents(object,[events])  
So far you've learned how to get elements with aliased query selectors, list the functions that will get invoked on events. But once-a-while there will come a time when your event will get triggered when it's not supposed to causing all kinds of weird behaviors. For that very instance you can use `monitorEvents()` function. This does exactly what it says. It simply monitors the events and logs whenever the event gets triggered on the specified element. Looking back to our earlier example let's now monitor events on that click now.
`monitorEvents(document.querySelector('testme'), 'click');`
This will log out Mouse Event object. According to the Paul Irish which was posted on [this](http://www.briangrinstead.com/blog/chrome-developer-tools-monitorevents) post.

> If the second argument is ‘mouse’, you’ll get all “mousedown”, “mouseup”, “click”, “dblclick”, “mousemove”, “mouseover”, “mouseout”, “mousewheel” events

> If the second argument is ‘key’, you’ll get all “keydown”, “keyup”, “keypress”, “textInput” events

> If the second argument is ‘touch’, you’ll get all “touchstart”, “touchmove”, “touchend”, “touchcancel” events. (See emulate touch events in devtools settings!!)

> If the second argument is ‘control’, you’ll get all “resize”, “scroll”, “zoom”, “focus”, “blur”, “select”, “change”, “submit”, “reset” events.

> If you don’t define a second argument, you’ll get all of the above, plus… “load”, “unload”, “abort”, “error”, “select”, “change”, “submit”, “reset”, “focus”, “blur”, “resize”, “scroll”, “search”, “devicemotion”, “deviceorientation” .

The comment above should sum up the monitorEvents for you if not then here's the official docs on:-

* [Chrome](https://developer.chrome.com/devtools/docs/commandline-api#monitoreventsobject-events)
* [Firefox](https://getfirebug.com/wiki/index.php/MonitorEvents#Event_groups) (even though it's doc for firebug, I think they're fairly similar)


####unmonitorEvents(object, [events])
And if you want to stop monitoring for events then you can just use this function to stop listening. Also, you can stop listening to any one specific event if you were listening to multiple events. An example:-
`monitorEvents(document.querySelector('.testme'), 'mouse')`
`unmonitorEvents(document.querySelector('.testme'),'mouseover')`

###copy
Often times whenever I'm trying to debug `json` objects I have to copy the response or just have to search if the key exists or not. Sure I could just do `key in jsonResponseObj` or other techniques but this is about copying the values from console to your clipboard. Simple, just do :-
`copy(anything-that-you-want-to-copy)`
That's all there is to it.

###keys(obj)
If you want to just list out the keys instead of checking it with `in` keyword then you can just `keys` function to list out all keys in your object.
keys({'hey': 'there', 'how':'you'}); // ['hey', 'how']

###values(obj)
Opposite to `keys`, `values` is used to list out all the values in an object.
`values({'hey': 'there', 'how':'you'})` _//will result in ['there', 'you']_ 
You can simple do the following to check if certain value exists or not if you want something similar to `in` for values.
`values({'hey': 'there', 'how':'you'}).indexOf('hey') > -1` _//true_


Besides mentioned above there are lot of things that console API is capable of doing simply because it is quite extensive and also because the docs are much more better at it. Also the API has slight variation on different browsers. Maybe in future I'll point out some of the most useful functions but for now here are the docs.
[Firefox Console API](https://developer.mozilla.org/en-US/docs/Web/API/console)
[Chrome Console API](https://developer.chrome.com/devtools/docs/console-api)

In my next post I'll be talking about `Debugger panel`. Feel free to raise issues if you see something glaringly wrong in this post. 
