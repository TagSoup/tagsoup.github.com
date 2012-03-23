---
layout: post
title: "Kitty Corners"
date: 2012-01-26 18:23
comments: true
categories: [CSS, level-up, dribbble challenge]
author: Tony Camp
---

<div id="centerer">
    <div class="corners">
        <div class="shadow">
            <img src="http://www.placekitten.com/256/300" alt="meow" />
        </div>
    </div>
</div>

Here is the end result of this experiment <a href="http://jsfiddle.net/tonyjcamp/rATuB/18/" target="_new">Kitty Corners</a>. Continue on to read my blabber.

As developers, we're constantly trying to "level-up". Becoming a better developer is all about writing code. A goofy, but fun exercise I do from time to time involves browsing <a href="http://www.dribbble.com" target="_new"/>dribbble</a>. I browse some of the great pieces of art on there and try to emulate it with CSS. I came across a piece by <a href="http://dribbble.com/larrychen" target="_new">Larry Chen</a> called <a href="http://dribbble.com/shots/118282-Photo-Border-Style" target="_new">Photo Border Style</a>. I figured I could whip up that border treatment pretty easily.

<!--more-->

I knew <a href="http://css-tricks.com/pseudo-element-roundup/" target="_new">CSS pseudo elements</a> would be necessary. I had seen a couple of blog posts showing how to manipulate css borders to make triangles <a href="http://www.russellheimlich.com/blog/pure-css-shapes-triangles-delicious-logo-and-hearts/" target="_new">(like this one)</a>. I figured I would just toss a :before and :after pseudo element on the image and get a nice start on my border treatment. 

### <a href="http://jsfiddle.net/tonyjcamp/rATuB/" target="_new">Snag #1</a>

Now, I messed with that pseudo element for a while and nothing was working. I thought I was going crazy. I wasn't, <a href="http://stackoverflow.com/questions/6949148/css-after-not-adding-content-to-certain-elements/6949190#6949190" target="_blank">pseudo elements can't be used on image tags</a>?!? Why? :before and :after only work with non-replaced elements. A replaced element is any element whose appearance and dimensions are defined by an external resource. So, yup, an img falls under this category. So my first idea is a no go... moving on.

Once I found out I couldn't use the pseudo elements on the img tags, I plopped in some span tags and added my pseudo elements. I had a white triangle overlapping the top left and bottom right corners. I was getting somewhere.

### <a href="http://jsfiddle.net/tonyjcamp/5J2LZ/1/" target="_new">Level +1</a>

The next task is getting the hairline drop shadows on the corner treatments. After thinking a bit, I had an easy solution. I would just add another triangle underneath it. This would be a tad bigger and blacker, but with some opacity. It turned out, the idea worked nicely.

### <a href="http://jsfiddle.net/tonyjcamp/rATuB/2/" target="_new">Level +1</a>

As you can see, the CSS is pretty messy. Lots of repetition, no inheritance, it was mostly just idea barf. Which is ok, but not final. More importantly, the damn thing doesn't scale...

### <a href="http://jsfiddle.net/tonyjcamp/x9knt/" target="_new">Snag #2 - Y U NO SCALE BRO?</a>

Adding the image to scale and keep the border treatment wasn't too bad, just change the display property on the container div
```
display: inline-block;
```

Now comes the markup and the CSS clean up. Here's where I ended up.

### <a href="http://jsfiddle.net/tonyjcamp/rATuB/18/" target="_new">Level +1 - FINISH</a>

I changed a few elements around. I added a div to center the image in the fiddle (totally not needed). Had one div with the psuedo elements for the top left and bottom right corner treatment, as well as a "shadow" element for each. In the end, 2 divs, and an img tag did the trick.

<div class="clearfix">
{% img left http://2.bp.blogspot.com/-HmFBQC3udcU/Tc9tNlBmfyI/AAAAAAAABZQ/e5F6o4XmEMQ/s400/jambi.jpg 60 %}
{% img left http://2.bp.blogspot.com/-HmFBQC3udcU/Tc9tNlBmfyI/AAAAAAAABZQ/e5F6o4XmEMQ/s400/jambi.jpg 60 %}	
{% img left http://2.bp.blogspot.com/-HmFBQC3udcU/Tc9tNlBmfyI/AAAAAAAABZQ/e5F6o4XmEMQ/s400/jambi.jpg 60 %}
{% img left http://2.bp.blogspot.com/-HmFBQC3udcU/Tc9tNlBmfyI/AAAAAAAABZQ/e5F6o4XmEMQ/s400/jambi.jpg 60 %}
{% img left http://2.bp.blogspot.com/-HmFBQC3udcU/Tc9tNlBmfyI/AAAAAAAABZQ/e5F6o4XmEMQ/s400/jambi.jpg 60 %}
</div><!-- / -->

