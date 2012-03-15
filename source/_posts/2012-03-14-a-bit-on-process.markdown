---
layout: post
title: "A Bit on Process"
date: 2012-03-13 08:31
comments: true
categories: [QA, Shut Up Truett, Writing Challenge]
author: Truett Kueck
---

###Day 19 of the 30 Day Writing Challenge

I can't think of anything more uniformly despised by everyone in my career than process.  Just mentioning the word generally causes a visible reaction.  That isn't a good thing.  A good process should be your best friend that makes everything else in your life easier.  I know why everyone reacts that way.  We've all worked somewhere with a rigid process whose endless meetings and documents detract more than they add.  That doesn’t mean process itself is bad, just that those processes are.
 
A process doesn’t have to be rigid.  It doesn’t even have to cover development lifecycle if you don’t want it to. If you notice a recurring problem, implement a rule to solve it.  Say, for example, you are regularly deploying sites and the links don’t function properly.   This is clearly a problem, and bad for you.  Now we identify what caused that problem.  In this example it was the page was never tested in IE7, and some of the fancy modern code doesn’t work there.  How was that missed?  We never identified what browsers the client expected the site to function properly in.  So we now have the rule,  “Identify all required support configurations from client prior to the start of development.” Congratulations, you have a process.  It’s that simple.  There is no one process that will solve every organization’s problems.   Every company is going to have to tailor their own process.
 <!-- more -->
The assumption that adopting a predefined process will work is a common mistake.  It’s an understandable one to make too.  The entire idea of process is that it should make your life easier.  What is easier than following a define set of steps?  However, just like you can’t copy and paste the same code from one project to the next, you can’t just copy someone’s process.  Their needs aren’t your needs.  So identify those needs.  Does management want greater visibility?  Are project managers asking you to add, “just one small thing,” to the sprint?  Our job as developers is to solve problems.  There is no reason we can’t solve our own. 
 
Before I end this, I will admit I’m biased towards process.  I’m a QA, without process my life is very hectic.  QA thrives in controlled environments.  If I’m given reliable inputs (Requirements Docs, Support Configurations, User Stories, etc.), I can’t guarantee the outputs of my testing.  I can assure you, without process requirements that those things exist, they won’t.  This leads incomplete testing, and thus quality problems.  No one likes those.   So help us help you better, create a process