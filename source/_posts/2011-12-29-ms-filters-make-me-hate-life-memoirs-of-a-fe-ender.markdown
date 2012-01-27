---
layout: post
title: "MS Filters Make Me Hate Life - Memoirs of a FE-ender"
date: 2011-12-29 14:29
comments: true
categories: [CSS, MS Filters, Gradients, Browser Wars, IE Hates Devs, Test Case]
author: Tony Camp
---

####Here's a screen shot of my issue. Notice the sub nav being cut off!

<div class="clearfix">
<a href="/images/nav-barf.jpg" target="_new">{% img left /images/nav-barf.jpg 300 197 'Why does' 'IE Hate Developers?' %}</a>
</div>

<a href="http://jsfiddle.net/tonyjcamp/eNr2M/" target="_new"/>TL;DR - Gimmie Solution</a>

Don't you hate it when you find yourself debugging some weird CSS issue knowing full well you've solved the same exact problem in the past? I catch myself doing this all the time. I'm being a bit hard on myself. It's not like I run into the same bug, project after project, failing to learn the solution. It's more like... "Shit I remember this bug, it happened to me 9 months ago, I can't remember how I fixed it back then. KKKKAAAAHHHHNNNNNNNN".

This just happened to me. Instead of repeating my same old pattern of "solve and forget" (devs love patterns), I decided to take some time to write about the bug and solution. Partly for you guys, but also for myself. Now I can always reference this blog post for the solution.

Spoiler alert, after creating the reduced use case, the issue wasn't what I originally thought it was.

###Situation:
I have a simple horizontal navigation menu with drop down sub-menus. This menu also has a background gradient. Totally basic, "every website has it" kinda stuff.

###The Issue I was having:
All was well in modern-browser-land, BUT, IE9 was DOM-barfing. For some reason, IE9 was giving me an effect similar to having "overflow: hidden;" on the containing nav element. Anything outside of this container (in my case, the sub-menus) were being cut off. So first thing I look for is the overflow, then the z-indexing, what the box is doing then, then, then, etc. until I start to cry QQ.

###Solving the issue:
Chris Coyer has a great artice on <a href="http://css-tricks.com/reduced-test-cases/" target="_new">creating a reduced test case</a>. Remembering the gist of it, I opened up js-fiddle and started hacking away. I was pretty sure the filter: property was being the hooker in my code. I knew it was depricated these days, but whatevs. It ended up coming down to 3 CSS properties living on the same element. Those being:
```
position: fixed;
z-index: 9999;
filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#cccccc', endColorstr='#000000');
```
The filter BS jumped out at me right away. So I removed filter, and the content outside of the container was showing properly. The style became:
```
position: fixed;
z-index: 9999;
```

For fun, I then pulled out the position: fixed. The style became:
```
z-index: 9999;
filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#cccccc', endColorstr='#000000');
```
I had my gradient, and the z-indexing working! Nice! So I started blaming position: fixed and filter being together. I told all my friends about it and they were all "bro, so cool, let's shoot red bull into our vains and snort some vodka". Just for funsies, I removed the z-index but kept the position and filter on the element.
```
position: fixed;
filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#cccccc', endColorstr='#000000');
```
IT WORKED! No z-index, but WTF! I told all my BFF's about it and they cheered. More RB, vodka and badges to be had.

So wanting the best of all 3 worlds, the simpliest solution I came up with was to wrap the nav bar in container element (don't care which you use), and put position: rel and z-index on the container. I kept the position: fixed and filter on the nav. Everything worked great. WIN!
```
#brotainer {
    position: relative;
    z-index: 9999;
}

#ie-hates-us {
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#cccccc', endColorstr='#000000');
    position: fixed; 
/* This seems to break everything if you use z-index here, so use it on a container
    z-index: 9999;
*/
}
```
Here's the final fiddle:
<a href="http://jsfiddle.net/tonyjcamp/eNr2M/" target="_new">My final answer</a>

And dudes, I learned a ton by coming up with this silly use case and writing an even sillier blog post about it. If you ever find yourself solving and issue you want to remember later, write a post! Hell, just fork this blog and write a post. I'll add it.









<div class="clearfix">
{% img left http://26.media.tumblr.com/tumblr_lvsjfjCEMw1qza249o1_400.jpg 256 381 'Smart Dudes' 'Say Smart Things' %}
</div>