---
layout: post
title: "Pest Control 2: Electric Boogaloo"
date: 2012-03-06 22:36
comments: true
categories: [Writing Challenge, QA, ruby, Shut Up Truett]
author: Truett Kueck
---
###Day 12 of the 30 Day Writing Challenge:
Hello, I'm Truett Kueck and you're not.  Today I am going to expand on what I wrote about last week.  It is amazingly common to forget to add urls to links.  This is completely understandable.  You are developing the page in sprints, and sometimes the page you need to link to does not exist yet.  Multiple sprints later when that page exists it slips your mind and then BAM! a site is live with scores of non-functioning.  It is also possible, and very common, that the actual requirements for links are unclear or completely non-existent.  

To solve this problem I have developed a simple Ruby application that parses a page for any links and then generates a Selenium script that clicks through the links on the page.  You will still need to make a few modifications, but you are 95% of the way there.  It uses the mechanize gem, another of many web automation gems, so we will need to install that first.

 
```ruby
sudo gem install mechanize
```
 
Mechanize is actually completely capable of performing the functionality of the Selenium script, but it has one major disadvantage.  The Selenium script can be run against Saucelab's [OnDemandService][0] which gives you access to most every major OS/Browser combination.  That might not matter to you as a developer, but Compatibility Testing is the bread and butter of QA.  Now onto the code:

<!--more-->

```ruby
require 'rubygems'
require 'mechanize'

puts 'Please enter the url you\'d like to create a Selenium script for.'
url = gets.chomp
puts 'What would you like to name the script?'
filename = gets.chomp

agent = Mechanize.new
page = agent.get(url)
script = File.open(filename + '.rb', 'w')

pageLinks = []
page.links.each do |link|
	pageLinks.push(link.text)
end

script.puts 'require \'rubygems\''
script.puts 'require \'selenium-webdriver\''
script.puts
script.puts 'page = Selenium::WebDriver.for :firefox'
script.puts 'page.get(\'' + url + '\')'
script.puts

i = 0
pageLinks.each do |link|
	script.puts 'link' + i.to_s + ' = page.find_element(:link_text, \'' + link + '\')'
	script.puts 'link' + i.to_s + '.click'
	script.puts 'if page.title != PUT PAGE TITLE HERE'
	script.puts '	puts \'Test failed\''
	script.puts 'else'
	script.puts '	puts \'Test Passed\''
	script.puts 'end'
	script.puts
	script.puts 'page.quit'
	script.puts 'page.get(\'' +url +'\')'
	script.puts
	i += 1
end
```

The user is initially prompted to enter the url they would like to run against.  This does require the url to be entered in http://www.url.com format, but you can add in error checking fairly easily.  They are then asked what they would like to name the script.  Then magic happens and we end up with a nearly ready-made Selenium script.  Now we need to update the script to actually check that the links are working.  The outputted code for a given link should look like:

```ruby
link0 = page.find_element(:link_text, 'Images')
link0.click
if page.title != PUT PAGE TITLE HERE
	puts 'Test failed'
else
	puts 'Test Passed'
end

page.quit
page.get('http://www.google.com')
```

The angry caps locked, "PUT PAGE TITLE HERE," is our target.  As I mentioned in the previous article: Selenium is capable of checking for things on the page other than the title.  If the link goes to content that is hidden, you can check for specific text.  Whichever you use replace that text with what you are looking for, and you are done.  Now go fourth and fix broken links.

[0]:http://www.saucelabs.com