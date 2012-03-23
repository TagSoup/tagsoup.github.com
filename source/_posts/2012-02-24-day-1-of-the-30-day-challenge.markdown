---
layout: post
title: Day 1 of the 30 Day Challenge
date: 2012-02-24 20:24
comments: true
categories: [Writing Challenge, level-up, community]
author: Tony Camp
---

The idea is to have 30 days (in a row) of blog posts. The authors may vary. Subjects might not be serious; they don't need to be. The goal is to get out of our comfort zone and put something out there in hopes of helping / inspiring someone else. Most important, to have fun.

We need more people to join and post. You can see we have lots of space available. Here's [our calendar][0] showing who's posting and when. If you're interested shoot a msg to [@phxtagsoup][2] on twitter.

Now that the boring stuff is out of the way, here's a tiny "ahhhhhh" tip I can share. It's super basic, but I hear people ask this question all the time.

<a href="/images/undef-jams.png" target="_new">{% img left /images/undef-jams.png 800 'Undef' 'Jams' %}</a>

So I have a basic function inside of Chrome's console. When I press enter, I get the "undefined" msg. I haven't done anything though, so why the "undefined"? Javascript functions ALWAYS return a value. If you don't specify what it is, then it returns "undefined". YAY!

<!--more-->

If you would like a more elegant explanation, check out the ever useful, [Stack Overflow][1].

Since we're on the topic of `return` values... let's go over another small trick. Again, it's nothing mind blowing or revolutionary. I'm sure you've seen this pattern before (assume hondaElement is a class on an "a" tag:
{% codeblock Honda Elements lang:javascript %}
$('#coolElement').delegate('.hondaElement', 'click', function(e) {
	// do something silly here
	e.preventDefault();
});
{% endcodeblock %}
preventDefault... well, read that out loud and I bet you can guess what it's doing.

And here's another common pattern that is used:
{% codeblock Return Fasly lang:javascript %}
$('#coolElement').delegate('.hondaElement', 'click', function() {
	// do something silly here
	return false;
});
{% endcodeblock %}

So, both examples stop the default action of the `a` firing when clicked. The second example takes things a bit further. It not only stops the default action from firing, it also stops event bubbling. It's the same as doing:

{% codeblock Same as doing this lang:javascript %}
$('#coolElement').delegate('.hondaElement', 'click', function(e) {
	// do something silly here
	e.preventDefault();
	e.stopPropagation();
});
{% endcodeblock %}

Now, the caveat... I believe this only applies to jQuery. Vanilla js still needs to have both `e.preventDefault()` and `e.stopPropagation()`. If anyone has some extra info on that, please, leave a comment and set the record straight.

[0]: https://www.google.com/calendar/b/0/embed?src=6tbdk1sbit95epi2bb8c8jqqv4@group.calendar.google.com&ctz=America/Phoenix
[1]: http://stackoverflow.com/questions/903120/should-i-always-give-a-return-value-to-my-function/903126#903126
[2]: https://twitter.com/#!/phxtagsoup