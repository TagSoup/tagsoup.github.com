---
layout: post
title: "QA - Pest control for development"
date: 2012-02-29 21:26
comments: true
categories: [Writing Challenge, QA, ruby, Shut Up Truett]
author: Truett 
---

###Day 6 of the 30 Day Writing Challenge

I, unlike most of the Tag Soup group, am not a developer.  I’m currently a Web Tester at [melt media][0] and have held other QA titles at other organizations. QA is here to help you make better and higher quality products.  This is usually accomplished through two methods: manual testing and automated testing.  This post will concentrate on providing an introduction to automated testing using Selenium and Ruby.  Foreword before we get to any code: I’m still new to writing Ruby code.

First you’re going to need to have Ruby installed.  If you don’t already, follow [this guide][1].  Once that is accomplished you’ll need to install the Selenium webdriver gem.
 
```ruby
Sudo gem install selenium-webdriver
```
 
One more note before we get further into the code.  Automation is not a replacement for manual testing.  Anyone that tells you this is lying.  This post is going to cover using Selenium to automate link verification on marketing sites.  This seemingly innocuous test is very critical, and often overlooked in manual testing.
 <!--more-->

```ruby
require ‘rubygems’
require ‘selenium-webdriver’

Driver = Selenium::Webdriver.for :firefox
```

This will create the object Page, which at this moment is a blank firefox window.  Loading any specific page is done by calling the Get method on that object.
 
```ruby
url = ‘http://www.meltmedia.com’
Driver.get(url)
```

Your browser should now be on the meltmedia homepage.  Now let’s make objects for each link.  I’m going to be finding the elements by the link text.  You should truthfully use :id, :class, or xpath where applicable as they’re less likely to change than the copy of a page.
 
```ruby
workLink = Driver.find_element(:link_text, ‘Work’)
workLink.click
```

You should now be on Melt’s portfolio page.  Now we have to check that the proper page loaded.  You can do this a number of ways.  You can search for specific elements, check the page title, or look for specific copy.   At the same time I’ll have the test output the results to the terminal.  You can output to a text file if you’d like as well.

```ruby
if Driver.title != ‘Meltmedia – Our portfolio of client work’
  puts ‘Work  link failed to function properly’
else
  puts ‘Work link functioned properly’
end
```

We now have a simple test that verifies a link.  We can use this test as a template for other link verification tests.  Either by making one long script that runs through ever links, or scripts for each individual links.  I prefer the later because it gives you granular results and lowers the chance of failure.  Since each test is independent of the previous, one test failing can’t cause another test to fail as well.
 
If you’d like to look up additonal information on Selenium please see [selenium hq][2] and the [googs code][3]

<div class="clearfix">
{% img left http://s2.hubimg.com/u/4746909_f520.jpg 60 %}
{% img left http://s2.hubimg.com/u/4746909_f520.jpg 60 %}
{% img left http://s2.hubimg.com/u/4746909_f520.jpg 60 %}
{% img left http://s2.hubimg.com/u/4746909_f520.jpg 60 %}
{% img left http://s2.hubimg.com/u/4746909_f520.jpg 60 %}
</div><!-- / -->


[0]: http://meltmedia.com/
[1]: http://www.ruby-lang.org/en/downloads/
[2]: http://seleniumhq.org/projects/webdriver/
[3]: http://code.google.com/p/selenium/