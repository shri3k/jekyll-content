---
layout: post
title:  "exports vs module.exports"
date:   2014-06-29 01:34:19
categories: casual node modules
---

I've been out for quite some time not just two weeks as I had mentioned in my last post. I decided to take a break from coding and travel a bit. I now officially love Boston, in summer atleast. Of course this is not relevant to what the topic at hand is but if you were like me who spends pretty much his entire life in front of computer then I encourage you to go travel and see new places. It will definitely open up your mind to new things. With that insightful words about life, let's now dive into what exports and module.exports actually does.

I'll assume that you already know basics of how NodeJS uses modules, if not then I highly recommend taking a look at [CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1.1) module which NodeJS follows.  
So, you've seen people use both `exports` and `module.exports` while exporting their module. Before that, let's first uncover on what really gets exported.

###What really gets exported
Quite simply, `module.exports` is the only thing that gets exported. So how exactly does `exports` export the module? 

###exports is just a global reference to module.exports
If you understand how referencing works in JavaScript then this is straight forward. `exports` is just pointing `module.exports` which is why you cannot reference functions or any other objects to it, if you do, then whatever that you were trying to export will simply not work because the reference has been broken. Let's look at an example:-

    function Calculator(x,y){
    	this.x = x;
    	this.y = y;
    }
    Calculate.prototype = {
    	constructor: Calculator,
    	add : function(){
    		return this.x + this.y;
    	},
    	subtract: function(){
    		return this.x - this.y;
    	}
    }

    exports = Calculator; // This will not work.

Don't worry about the code so much except the last line. The reason that it fails, as said above, is because it's now referencing the `Calculator` function and since `module.exports` is the only thing that gets exported `exports` just sits there not knowing what to do. However, do note that the following however works:-

    function Calculator(x,y){
    	this.x = x;
    	this.y = y;
    }
    Calculate.prototype = {
    	constructor: Calculator,
    	add : function(){
    		return this.x + this.y;
    	},
    	subtract: function(){
    		return this.x - this.y;
    	}
    }

    exports.Calculator = Calculator; //This will however work.

Think for a moment on why the above works. 

If you remove the veil of `exports` then the above code is essentially doing the following:-  

    module.exports.Calculator = Calculator;  

Why? Since you haven't really broken the reference of `exports` to `module.exports` and only adding properties to `module.exports` the above code should work.

If you really want to use `exports` and be able to reference it to your function then you can also do this:-  

    module.exports = exports = Calculator;

I hope that clarifies a little on `module.exports` and `exports` to you. It's just a little concept but I had the hardest time figuring out why some uses one over another. `module.exports` is more elegant way to export the module because it's "OOP" way. Don't take my word for it that's what people say. :)


