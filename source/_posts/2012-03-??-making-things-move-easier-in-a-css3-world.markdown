---
layout: post
title: "Day ?: Making Things Move Easier in a CSS3 World"
date: 2012-03-?? 21:58
comments: true
categories: [css3, javascript, coffeescript, animation]
author: Jim Jeffers
---

CSS Animations have been around for a few years now but when they were first introduced you could only use them in Safari (well webkit technically, although they ran at like 9 fps in Chrome in the early days). That didn't stop me from making these [sumo wrestlers](http://sumocreations.com) but it did make them a novelty that most people couldn't see. Fast forward to today - Chrome, Firefox, and the IE10pp4 all support hardware accelerated CSS animations. Browser support is already today, especially on mobile, [and it will only get better](http://caniuse.com/#feat=css-animation). So why not start using them now? Declaring animations in CSS requires a lot of code, and supporting multiple browsers means repeating a lot of this lengthy code many times over. Here's how to cope:

## Dealing w/ Prefix Hell

Using CSS animations can be a nightmare. Browser prefixes on properties is one thing, but with CSS animations we're talking about entire prefixed blocks of code containing prefixed properties. No big deal right? Take a look at how much code it took to setup the 'rocking' animation I applied to some of my sumo wrestlers:

<script src="https://gist.github.com/1883460.js?file=sumo_balancing.css"></script>

It takes 15 rules to properly apply and configure the animation on a given element. That's inconvenient enough -- but then it takes a whopping 74 lines of code to define a simple animation with only 3 keyframes. So how do we solve this?

Pre-parsers to the rescue? Not the case - at least not yet. I haven't seen or found a way to write a SCSS mixin that could handle blocks. I'm fairly sure mixins are not designed to generate entire blocks of CSS nor were they ever intended to do so. Pre-parsers need another solution on top of functions and mix-ins to handle the problem presented by keyframe declarations.

## Generating CSS with Javascript

There are some interesting tools out there for this. [Prefix free](http://leaverou.github.com/prefixfree/) is one of the most ambitious ideas I've seen yet. But most of us probably don't want to load our CSS twice. Becuase web browsers ignore CSS rules they don't support, you can't access them via the DOM. To get around this, scripts like prefix free load the CSS file via an XHR request and parse the entire CSS file in javascript. It's a cool idea and definitely worth checking out, but not necessarily for everyone.

Another option is to simply use javascript to generate CSS when you need it. The downside is some presentation code normally reserved for CSS is now stored in javascript. This too is not for everyone, but I find it to be a justified trade off in the case of CSS animations where the alternative is writing hundreds of lines of throw-away pre-fixed code.

To demonstrate how you can do this, I created a small class in CoffeeScript called Animator:

<script src="https://gist.github.com/1883460.js?file=animator.coffee"></script>

Now, to define that 'rocking' animation that took 74 lines of CSS, we can just define our keyframes in JSON:

<script src="https://gist.github.com/1883460.js?file=ready.coffee"></script>

The generate method will append the following CSS to the document's last stylesheet:

<script src="https://gist.github.com/1883460.js?file=generated_animation.css"></script>

The Animator.animate() method will apply the browser prefixed properties for the animationName, animationDuration, animationDelay etc.. All of the prefix handling here is done via [Modernizr.prefixed()](http://www.modernizr.com/docs/#prefixed). If you're interested in trying it out [I've posted the source to github with a quick html file to demo](https://github.com/jimjeffers/Animator.js).

The important thing to gain from this is that CSS Animations are  pretty well supported but difficult to maintain. Like all proprietary properties, managing all of these prefixed rules is a pain point for developers and CSS animations are probably one of the best examples of how much this can get out of hand.

**One more thing: if you're fascinated by generating CSS animations with Javascript, checkout my other Coffeescript project, [Sauce.js](https://github.com/jimjeffers/Sauce) which generates complex tween transitions (elastic, back, bounce) through CSS animations.