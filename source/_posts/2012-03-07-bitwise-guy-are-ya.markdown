---
layout: post
title: "Bitwise Guy Are Ya?"
date: 2012-03-07 21:04
comments: true
categories: [Writing Challenge, javascript, Operators, level-up]
author: Tony Camp
---

###Day 13 of the 30 Day Writing Challenge

The idea for this post all started with a friend of mine, [Connor McSheffrey][0] (@mcsheffrey). I don't normally see a whole lot of activity from Connor on github. This morning, he posted a short gist:

{% gist 1991635 %}

I took a look at noticed something I didn't understand. The &#126;&#126; operator. So, I had to start researching. For whatever reason, Google doesn't recognize &#126;&#126; in the search engine. I tried surrounding it with quotes etc and nothing relavent came up (please let me know why)! I knew this was an operator, so I started searching for operators. Sure enough, I found the ~ operator. I didn't even know this one existed. Do you know what this does? Let's break it down:
<!--more-->
```javascript
~8 //-9
```
The ~ operator takes an integer, inverts the value, and subtracts 1 from it. In our case, 8 becomes -8, and -8 + -1 === -9. A "nice to know" but this didn't answer my question. I continued my search, this time for "bitwise operators". After buttload of searching, I found this awesome [JS Perf presentation by Thomas Fuchs][1].

A golden rule, that Thomas emphasizes is "DO NOT, EVER, OPTIMIZE PREMATURELY". Sage advice. In our case, we're not researching this bitwise operator because we need to micro perf our scripts. It's more like a "WTF is Connor doing here, I need to know so he doesn't call my dumb ass out next time I see him", sort of a thing. So next time you run into Brenden Eich and ask him, "You invented JavaScript" and he replies "~~(true)", you know what it means. 

Back to the Thomas Fuchs slide share. He brings up this operation:

```javascript
parseInt(12.5) //returns 12
```

and the faster:
```javascript
~~( 1 * "12.5" ) // also returns 12
```

Side note: Thomas doesn't need the whole 1 * "12.5", it can just be "12.5"

###Now we have a name for our operator: the bitwise Double Not (~~).

Before we get back to Connor's script, let's break down the Double Not. Here's a couple of examples:

``` javascript

~(1) // inverts the value, and adds (-1), so (-1) + (-1) === -2
~(-2) // inverts the value, and adds (-1), returning (2) + (-1) === 1
~~(1) // returns 1
```

###There is one more thing the Double Not does for us.
Damn you James Padolsey for being so smart. He [blogged about the Double Not][2] over a year ago and added this little nugget:<br />
&nbsp;" ~~‘s flooring capabilities make it a better alternative to Math.floor if you know you’re dealing with positives — it’s faster and takes up less characters. "

So, it's a micro perf for Math.floor (in some browsers)! Nice nugs dude!

### I'm hoping this is looking a little familiar. 0's and 1's, falsy and truthy... ahhh!!!

``` javascript
~~(true) // returns 1
~~(false) // returns 0
typeof ~~(true) // returns "number"
```

Back to Connor's gist:

{% gist 1991635 %}

Depending on the string match of "next" we get a boolean value *cough* carousel nav *cough* AND we will +1 to `this.current` or -1 from `this.current`. It works perfectly with going to the next picture, or the previous picture in a photo carousel.

These sites were exremely valuable when researching Connor's jizzst!<br />
[Thomas Fuchs's Extreme Javascript Performance][1]<br />
[James Padolsey's Double bitwise NOT (~~)][2]<br />
[MDN's Old BitWise Operators Page][3] - cached version, original is a dead link<br />
[tuts+ - Where Connor originally saw this][4]<br />


...another fun thing this research has taught me, typing &#126;&#126; and another &#126;&#126; in octopress is the same as using `<del></del>` tags. Yay.

Special thanks to Connor for posting a single, simple gist that got me going on this whole subj. Kisses!!!

<div class="clearfix">
	{% img left http://25.media.tumblr.com/tumblr_lyc00d4cdV1qzfebyo1_400.gif 50 %}
	{% img left http://25.media.tumblr.com/tumblr_lyc00d4cdV1qzfebyo1_400.gif 50 %}
	{% img left http://25.media.tumblr.com/tumblr_lyc00d4cdV1qzfebyo1_400.gif 50 %}
	{% img left http://25.media.tumblr.com/tumblr_lyc00d4cdV1qzfebyo1_400.gif 50 %}
	{% img left http://25.media.tumblr.com/tumblr_lyc00d4cdV1qzfebyo1_400.gif 50 %}
</div>

[0]: https://twitter.com/#!/mcsheffrey
[1]: http://www.slideshare.net/madrobby/extreme-javascript-performance?from=fblanding
[2]: http://james.padolsey.com/javascript/double-bitwise-not/
[3]: http://webcache.googleusercontent.com/search?q=cache:eps2pgQRbloJ:tr.im/bitwise+&cd=1&hl=en&ct=clnk&gl=us
[4]: http://tutsplus.com/lesson/prototypal-inheritance-and-refactoring-the-slider/