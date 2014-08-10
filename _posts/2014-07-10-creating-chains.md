---
layout: post
title:  "Creating chains "
date:   2014-07-10 01:34:19
categories: javascript protip
comments: true
---

Just a quick tip this week. Although, most of you know about `this` already, you can return your prototype function with `this` to make them chainable.
Below is the snippet that will enable that:-
<script src="https://gist.github.com/sinkingshriek/f12bb33a19e383c3ec53.js"></script>

This is technically what jQuery uses for their functions to be chainable and I just recently found out about. Makes your code lot more dynamic this way.
