---
layout: post
title: "Day 4: Drunk and Pushing State"
date: 2012-02-27 21:58
comments: true
categories: [pushstate, html5, drunk, javascript]
---

In the immortal words <del>or</del> <em>of</em> Colonel Sanders, I'm too drunk to write this blog post. But eff it (note: I don't know if Colonel Sanders said that part). Don't take this as a knock that I don't care about this blog post because I do. The HTML5 history API is one of my favorite things ever (and Tony Camp is one of my personal heroes). If the HTML5 history API (or Tony Camp) came up to me and said, "Hey baby, I work to spec in all the latest browsers and degrade gracefully in the others. What are you doing later?" I would say, "Writing some code. See you later." I love it so much, I gave a talk about it at a previous [Tag Soup meetup](http://tagsoup.github.com/blog/2011/10/28/html5-history-api-with-luke-karrys/). But here I am going to tell you how to convert your current Octopress blog into an Octopress blog that uses HTML5 pushState (and how to report bugs to me where I messed up, because I definitely did).

<!--more-->

But seriously I think the 4 turkey burgers (trying to watch my figure) I ate tonight match up well with the 5 vodka sodas (again, with the figure) I drank tonight, so we should be good to go to learn about some sweet, sweet HTML5 history.

## Can I use this?

Short answer: yes. Long answer: maybe, but it's complicated. The history API is great. Remember hashbangs (#!)? Of course you do, they are still in use at some big-time websites (including Twitter). [That was a mistake](http://storify.com/timhaines/hashbang-conversation). But there is a much better way. The history API enables you to change the entire URL (as long as it is the same domain) when you AJAX in some sweet new content. The part that is complicated goes by the ugly name of "partial browser support".

I'm sure you all love feature detection. And I'm also sure that you love not being lied to. Now imagine if your feature detects lied to you. You would probably be upset. We all would. No one likes being lied to. So when Safari 5.1 is like "Oh what's up Modernizr. The history API? I support that. No problem. Throw it at me." You are gonna feel all warm and fuzzy inside until another supported browswer says "Oh yeah, I support that as well. But I do it differently than Safari. That browser is dumb. Seriously, that browser is so dumb it got hit by a parked car." And you're going to say, "Come on browsers. Can't we all just get along. Don't let native apps win!"

But there is a solution. Benjamin Lupton wrote [History.js](https://github.com/balupton/History.js/). It has [bugs](https://github.com/balupton/History.js/issues?sort=comments&direction=desc&state=open) (including a bug where its pushState method won't accept a URL with a hash which I have hit multiple times), but it also solves a lot of browser inconsistencies and it has tests. So I use it. This is a case where I don't want to reinvent the wheel, so I'm going to use a well tested library that someone else has written. Yes, some of the bugs are a pain, but I'd rather start a project with it than without it.

## Show me some code!

Oh yeah, sorry about that. I ramble when I'm drunk. So Octopress, you use it, you love it. It is static and fast. But you want to bypass the full page reloads. I have a solution for you (and I could use your help on improving it, see the last paragraph). Here is the basic flow that we want to follow in order to get some sweet JS that does what we need it to do:

1. Capture internal link clicks
2. Prevent the default action
3. Use HTML pushState to register the interaction
4. Capture that interaction by listening to an event
5. Change our content
6. Execute any other miscellaneous JavaScript to change the state of our page

Sound easy enough? It is!

## I just said show me some code and you rambled again. Seriously, show me some code.

Yes. I promise I will show you some code. Let's start with step 1. This is some code I yanked from [Benjamin Lupton](https://gist.github.com/854622) and tweaked a little to work a bit better with Octopress. This code is a jQuery helper to grab any internal links. The idea is that we are only going to want to attach our click handlers to internal links. The meat of this code is where we assign a boolean value to `isInternalLink`. We are checking whether the href of the link is equal to the root url or if it doesn't contain a colon. We also want to make sure that the href doesn't start with a hash (there it being a named anchor) and also isn't a link to the RSS feed.

{% codeblock %}
$.expr[':'].internal = function(obj, index, meta, stack) {
  var $link = $(obj),
      url = $link.attr('href') || '',
      isInternalLink;

  // Check link
  isInternalLink = ((url.substring(0, rootUrl.length) === rootUrl || url.indexOf(':') === -1) && url.charAt(0) !== "#" && url.indexOf('atom.xml') === -1);

  return isInternalLink;
};
{% endcodeblock %}

## Ajaxify?! I barely know her.

Next, we want to to use another method I yanked from Benjamin. This is enables us to run the ajaxify() function against jQuery objects. The only trick here, is the bug I talked about earlier. We have to separate the hash from the clicked url and pass it to the pushState event listener through the first parameter, which is a data object that can contain any arbitrary data.

Another note worth mentioning is the second parameter of the pushState method. That accepts a title for the new page, but as far as I know, no browsers support it. Also when using the `ajaxify` function in this way, we don't know the full title of the next page at this junction. This could be rearranged, but since the browsers don't support it, I don't see much reason for doing so <del>know</del> <em>now</em> (especially since we will update the title later).

{% codeblock %}
$.fn.ajaxify = function() {
  var $this = $(this);
  
  $this.find('a:internal').click(function(event) {
    var $link = $(this),
        url = $link.attr('href'),
        hash = url.split('#')[1],
        data = {};

    // Continue as normal for cmd clicks etc
    if (event.which == 2 || event.metaKey) {
      return true;
    }

    if (hash) {
      url = url.replace('#'+hash, '');
      data.hash = hash;
    }
    
    // Ajaxify this link
    History.pushState(data, null, url);
    event.preventDefault();
    return false;
  });

  // Chain
  return $this;
};
{% endcodeblock %}

## Shhhhhhh! Listen.

The history.js plugin exposes a `statechange` event to the `window`. If we listen for that event, we can capture any state changes and bend them to our will. Here is the code I am using on my blog.

{% codeblock %}
$(window).bind('statechange', function() {
  
  var State = History.getState(),
      url = State.url,
      stateData = State.data,
      hash = stateData.hash,
      relativeUrl = url.replace(rootUrl, ''),
      isHome = (relativeUrl === '/' || relativeUrl === '/index.html' || relativeUrl === "") ? true : false,
      classMethod = (isHome) ? 'addClass' : 'removeClass',
      dsScript = (isHome || relativeUrl.indexOf("blog/archives") > -1) ? 'count.js' : 'embed.js',
      homeClass = 'blog-index';

  $.ajax({
    url: url,
    dataType: 'html',
    success: function(data) {
      var $data = $(data),
          $content = $data.find('#content > div'),
          title = $data.filter('title').text(),
          $gists = $content.find('.gist-holder'),
          gistEmbeds = [];

      $gists.each(function() {
        var $gistHolder = $(this),
            url = $gistHolder.data('gist-url'),
            id = url.replace(/.*\.com\/([0-9]+)\.js.*/, '$1'),
            file = url.split('?file=')[1];

        gistEmbeds.push($.get('/embed-gist.php?id=' + id + ((file) ? '&file=' + file : ''), function(data) {
          $gistHolder.html(data);
        }));
      });

      $.when.apply($, gistEmbeds).done(function() {
        $contentArea.html($content.html())[classMethod](homeClass).ajaxify();
        addCodeLineNumbers();
        disqus_identifier = url;
        disqus_url = url;
        disqus_function(dsScript);
        twitter_sharing();
        $('title').text(title);
        _gaq.push(['_trackPageview', ((relativeUrl.charAt(0) === '/')?'':'/')+relativeUrl]);
        
        var currentPos = $body.scrollTop(),
            $nav = $('nav[role="navigation"]'),
            $scrollTo = (hash) ? $('#' + hash) : $nav,
            scrollTo = $scrollTo.offset().top,
            distance = (currentPos > $nav.position().top || hash) ? Math.abs(currentPos - scrollTo) : 0;
        
        if (distance > 0) {
          $('html, body').animate({
            scrollTop: scrollTo
          }, distance, function() {
            if (hash) window.location.hash = hash;
          });
        }
        
      });

    }
  });

  return false;
});
{% endcodeblock %}

`History.getState()` provides us with an object that contains the URL, title and data object from the link that was just clicked. Here is where it can get a bit hairy. HTML is HTML right? We should just be able to AJAX in the URL we want, parse out the specific content areas that interest us, and replace the old content areas with the new ones. Well, almost.

The only dynamic third-party JavaScript that inserts content on my blog are my gists. If you have other similar JavaScript on your blog, you may have to write some new code that does something similar.

For gists, the rub lies in where for each gist we include a script tag which write our code from GitHub to our page. This doesn't <em>work</em> that well dynamically since it uses `document.write`. When `document.write` is used after the page is already loaded, is empties the content of the page, and inserts its new content. Pretty selfish, right? Well `document.write` may be an asshole, but that's not the end of the world. I came up with a [server-side solution](https://github.com/lukekarrys/lukecod.es/blob/master/source/embed-gist.php) for my blog, which will take some URL parameters and fetch the gist HTML for you. Ben Nadel also came with a solution earlier this year that uses an iFrame to override the `document.write` method. I love his way of doing it, because it all stays client-side, but I think the server-side method is a bit safer (since you aren't overriding browser method).

In my method, I am locating any gists in the incoming HTML. If there are any, I ask my server-side script to supply me with the HTML for that gist. I am also pushing that result of that request (a JQuery Deferred object) to an array. Later I am asking that array to let me know when all of its gists have been loaded.

Once all gists have been loaded (or immediately if there were no gists), I take the new content, assign it an appropriate class and ajaxify its internal links (`$contentArea.html($content.html())[classMethod](homeClass).ajaxify();`). After that I run a whole bunch of JavaScript functions to make sure we handle all the interactions that will need to happen for each page.

## Any other interactions? Bueller? Bueller?

There actually is! In our mobile media query, we use a select to change pages. This won't work for us, since our code only captures the interaction of internal links `a` elements. We need to change the `getNav` function so the select will call pushState for us. My code is below.

{% codeblock %}
function getNav() {
  var mobileNav = $('nav[role=navigation] fieldset[role=search]').after('<fieldset class="mobile-nav"></fieldset>').next().append('<select></select>');
  mobileNav.children('select').append('<option value="">Navigate&hellip;</option>');
  $('ul[role=main-navigation]').addClass('main-navigation');
  $('ul.main-navigation a').each(function(link) {
    mobileNav.children('select').append('<option value="'+link.href+'">&raquo; '+link.text+'</option>');
  });
  $('ul.subscription a').each(function(link) {
    mobileNav.children('select').append('<option value="'+link.href+'">&raquo; '+link.text+'</option>');
  });
  mobileNav.children('select').bind('change', function(event) {
    if (event.target.value) { 
      if (!!(window.history && history.pushState)) {
        History.pushState(null, null, event.target.value);
      } else {
        window.location.href = event.target.value;
      }
    }
  });
}
{% endcodeblock %}

THe new part is where we are binding the change of the select. If we have the history API at our disposal then we will call pushState, leaving `window.location.href` as a fallback.


## Woooo! Almost done.

So far we have done almost everything we need to do. Except the last part about executing any miscellaneous JavaScript to keep our pages in working condition. You know what they say: 90% of the vodka takes 10% of the time to drink. Something like that. Anyway, here we go. In the code snippet above, you may notice functions called such as:

1. `addCodeLineNumbers()`
2. `disqus_function(dsScript)` including the prior setting of `disqus_identifier` and `disqus_url`.
3. `twitter_sharing()`
4. `$('title').text(title)`
5. `_gaq.push(['_trackPageview', ((relativeUrl.charAt(0) === '/')?'':'/')+relativeUrl])`

The first three items are functions I created in the JavaScript code so that the code line numbers, Disqus and Twitter embeds would all work properly. Below I'm going to share those three snippets to ensure that you can get that code working for your site.

The first function (`addCodeLineNumbers`) already exists as a global function in `octopress.js`. This is good (although polluting the global namespace is normally frowned upon) because we can just call the function to add line numbers to any gist we have available in our new content.

The second function pertains to the Disqus comments.

{% codeblock disqus.html %}
{% comment %} Load script if disquss comments are enabled and `page.comments` is either empty (index) or set to true {% endcomment %}
{% if site.disqus_short_name and page.comments != false %}
<script type="text/javascript">
      var disqus_shortname = '{{ site.disqus_short_name }}';
      var disqus_identifier = '{{ site.url }}{{ page.url }}';
      var disqus_url = '{{ site.url }}{{ page.url }}';
      {% if page.comments == true %}
        {% comment %} `page.comments` can be only be set to true on pages/posts, so we embed the comments here. {% endcomment %}
        // var disqus_developer = 1;       
        var disqus_script = 'embed.js';
      {% else %}
        {% comment %} As `page.comments` is empty, we must be on the index page. {% endcomment %}
        var disqus_script = 'count.js';
      {% endif %}
    var disqus_function = function (_disqus_script) {
      var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
      dsq.src = 'http://' + disqus_shortname + '.disqus.com/' + _disqus_script;
      (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    };
    
    disqus_function(disqus_script);
</script>
{% endif %}
{% endcodeblock %}

Here, the main point I changed, was to assign the IIFE that ran the Disqus code to a function expression so I could call it with the appropriate script when it needed to be. On the home page, I call it with `count.js` if we are on the homepage and `embed.js` if we are on a post page (and therefore embedding comment thread).

I also do the same thing with the tweet button.

{% codeblock %}
{% if site.twitter_follow_button or site.twitter_tweet_button %}
  <script type="text/javascript">
    var twitter_sharing = function(){
      var twitterWidgets = document.createElement('script');
      twitterWidgets.type = 'text/javascript';
      twitterWidgets.async = true;
      twitterWidgets.src = 'http://platform.twitter.com/widgets.js';
      document.getElementsByTagName('head')[0].appendChild(twitterWidgets);
    };
    twitter_sharing();
  </script>
{% endif %}
{% endcodeblock %}

I assign the code to a function expression, which I can then run at my convenience. Normally this type of code is meant to only be called on the initial loading of a page, but since we are accepting the role of Dr. Frankenstein and taking code and making it into our own monster, we have to make it callable from other places.

The last two things I am doing don't require code changes from anywhere else, but are important nonetheless. The first is to change the page title. This isn't the most important thing, but you know that feeling when you have tons of tabs open and you can't tell what it what based on the titles. Don't be hostile towards your users! Show them an accurate page title and they will be happy and come back to your blog to read about how awesome your cats are!

Lastly, I am telling Google Analytics to track the pageview. You know how important this is. The only trick I am pulling here, is to make sure that the URL I am sending to Google starts with a `/` since I believe that is required (not 100% sure about that).

## Wake up! I'm about to do a recap.

If you include the first three codeblocks on your pages and replace the other codeblocks (Disqus and Twitter) you should be good to go.

_But wait there's more!_

Your mileage may vary (YMMV). I know that sucks to hear but there is good news. I definitely did not cover the edge cases on this one. My blog doesn't have any videos or some of the other plugins. I would love to hear if you implement this code and find some JS that doesn't work. Seriously, tell me where I am wrong or where my code falls short!

Peace out, and if you don't chew Big Red, well it's spicy and you'll love it.

