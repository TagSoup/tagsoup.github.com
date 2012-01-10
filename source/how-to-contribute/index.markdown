---
layout: page
title: "How to Contribute"
date: 2011-12-04 22:35
comments: true
sharing: true
footer: true
---

{% img center /images/its-dangerous-to-go-alone-take-this.jpeg 400 250 'It IS Dangerous' 'Take This' %}

So you want to contribute. We're using Octopress to power our page. Pretty neat right, well there are some drawbacks. The main one being set up. If you're not famailiar with Ruby, Terminal or GitHub you might be intimidated. Don't be scared, we're here to help you, help us. So here's a small walk through that will get you a local version of the TagSoup octopress on your system.

Let's get the important links listed first. These will help you out tremendously. These links are where I found the info to get everything up and running smoothly.

<ul>
	<li><a href="http://octopress.org/docs/setup/" target="_new">OctoPress Offical Setup</a></li>
	<li><a href="http://beginrescueend.com/" target="_new">RVM (Ruby Version Manager)</a></li>
	<li><a href="http://progit.org/book/">Pro Git</a></li>
	<li><a href="http://think-like-a-git.net/">Think Like a Git</a></li>
</ul>



If you're a mac user, you want to make sure you have xcode installed. You can grab xcode from the app store. Once that's done, the next prerequisite is GIT. Make sure you have GIT installed, and some basic familiarity with it (read Pro Git and Think Like a Git). Once you have these installed and ready to go, follow the OctoPress Set Up instructions. When you get to the "Setup Octopress" subsection you're going to want to change the url for the repository to clone. You are going to want to run
```
	git clone git@github.com:TagSoup/tagsoup.github.com.git
	cd tagsoup.github.com
	ruby --version
```
Just like the install site says, you will want to see Ruby 1.9.2.

Now, before you run the gem install bundler, you want to make sure you're on the 'source' branch. You might have to create the branch first. You can run
```
git checkout source
```
This will create the branch and auto change to it. After this, you can continue with the install process by running:
```
gem install bundler
bundle install
rake generate
```

That's it! The install is done.

Now you can create a post and contribute to the blog. I'm sure some documentation on HOW to create a new octopress post might be useful. Head to <a href="http://octopress.org/docs/blogging/" traget="_new">http://octopress.org/docs/blogging/</a> for a quick guide. Once your post is created and you're happy with it, <a href="http://help.github.com/send-pull-requests/" target="_new">send me a pull request</a>!

If you need any help, or have questions, just shoot us a msg on github. Kthxbai
{% img /images/animated-kitty.gif 'OMG' 'SO THIRSTY' %}
