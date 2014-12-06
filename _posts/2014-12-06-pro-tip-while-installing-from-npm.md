---
layout: post
title:  "pro-tip while installing from npm"
date:   2014-12-06 01:34:19
categories: node npm pro-tip 
---

Usually, I start off my project with `npm init` to get the `package.json` manifest file in the current working directory but recently for a quick prototype I skipped that process and tried installing some packages when I noticed something peculiar. All my packages was getting installed in the parent/parent directory. After much research, I finally figured out that I had `package.json` on my parent/parent directory and that's why it wasn't installing in the current working directory. 

So pro-tip, start with `npm init` to not waste **30 MINUTES OF YOUR LIFE**
