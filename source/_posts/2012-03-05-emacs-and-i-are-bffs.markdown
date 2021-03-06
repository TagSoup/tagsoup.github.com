---
layout: post
title: "Emacs and I are BFF's"
date: 2012-03-05 08:45
comments: true
categories: [Writing Challenge, Text Editors, Tools, IDE, Emacs]
author: Nick Crohn
---

### Day 11 of the 30 Day Writing Challenge: A look at why I use 35 year old software

## Why Emacs?
Let me just start off by saying that this is by no means an intent to disuade you from using your editor of choice. That being said, stop using your bloated GUI IDEs. Okay, maybe that was a bit harsh, but do you remember when your teachers in school would say, "keep it simple?" It's true and I'm sure if you are slightly familiar with Emacs you are saying to yourself, "but emacs isn't simple, I couldn't even figure out how to get out of the program!"

_Note_ If you are on linux or Mac you should know these commands:

    vi: Save [ESC] w [Enter]
        Quit [Esc] q [Enter]

    emacs: Save [CTRL]-x s
           Quit [CTRL]-x c

I will fully admit there is a learning curve to Emacs, it's the biggest draw back of any of the terminal editors. When I first started using Emacs it took me around 2 months to be able to use it exclusively. Up until that point I was augmenting my development with the GUI editor I was using prior. My point though is, stick with it, make a cheat sheet, keep a page open of common commands, so on and so on. Your hard work will be rewarded with efficient, clean and simple development.

<!--more-->

## Keep Your Hands on those Keys!
Over the years I've come to know how critical it is to get an idea out when it's flowing. While working at a data entry job I picked up a skill that has helped me emmensly, even if you don't use Emacs. To flow faster, try and keep your hand off the mouse. Every transition slows you down and impedes your train of thought into the code. The best part about this is you can pracitce this with whatever editor you're already doing. Emacs, though allows you to maximize on keyboard use.

One of the biggest culprets of mouse use is text selection, Emacs has a way to do all of that with just the keyboard.

    Key Commands:
    Yank a line                        [CTRL]-k
    Place a yanked line                [CTRL]-y
    Select the entire document         [CTRL]-x h
    Drop a Mark                        [CTRL]-[SPACE]

Yanking is the equivalent of copy, it just doesn't end up in your clipboard. You can integrated droping a mark anywhere in the document and moving the cursor to where you'd like to yank and it will grab everything between the dropped mark and your cursor.

Emacs has a concept of yanked lines so you actually have a log of what has been yanked, but I won't get in to that here. Moving right along...

## Switching Windows Often?
I know when I'm doing setup for a new project I'll use the command line to create folders and structures on the file system. There are times though after setup and I'm working on a JavaScript file and realize I'm missing a folder I need. Emacs has a plethora of built in commands (over 2000 in fact) to do any number of things. For this instance it can make directories for you.

    How to Create a Directory
    M-x "make-directory" RET
    Type the path in the input and hit return

_(M is reference for the Meta key, on OSX the meta key is Esc)_

It's that simple and flows directy inline with your working environment.

## Can I get that Code Snippet in Perwinkle Blue?
Emacs is a LISP editor, it runs and interprets LISP to make it the powerhouse that it is today. What does that mean? Well, LISP is a functional language that was designed originally for processing lists. (LISt Processing). [Wikipedia](http://en.wikipedia.org/wiki/Lisp_%28programming_language%29)

Lisp provides Emacs it's power because it allows you to specify any new functionality you'd like your editor to have. Emacs has a concept of Major and Minor modes which are all just small Lisp programs. I'll give you an example of the features that can be provided.

- Colored syntax highlighting
- Smart code indentation
- Tag dictionaries
- Syntax error highlighting
- Etc.

One of my favorite major modes is [JS2 Mode](http://code.google.com/p/js2-mode/) by Steve Yegge. JS2 Mode makes editing JavaScript a dream. I won't go into specifics here, just take a look at the Google Code site and read for yourself on it's capabilities. One of the biggest things it has going for it is the error highlighting, which I will be talking about next.

## Error! You have Code Syntax Problems!
One of the banes of most JavaScript developers is the leniency of browsers fixing broken syntax in files. When you have lagging commas or missing semi-colons you have ticking time-bombs in your code. If you were looking for these areas by hand it would take a life time. Yegge's JS2 Mode has built in error finding and will show you live, while you are coding.

    Usage
    Skip to next error [CTRL]-x `

That command will jump you to the next error in the document and  show you a short message of what is wrong. For me, this single bit of functionality has saved me a number of hours of hunting down that elussive missing comma.

## I Don't Know Where to Start.
Now, I don't want to leave you hanging, so I'll point you to [my emacs github repo](https://github.com/ncrohn/emacs) where you can get my setup. This will give you a jump start on coding with Emacs. If you do start using Emacs, start making your own profiles and customize Emacs to just how you want it. Customization is the single most reason I'd recommend Emacs.

    Installation
    cd to the github clone
    cp .emacs ~/.
    cp -r vendor ~/.emacs.d/.

Once you are done there, go to terminal and try editing a JavaScript file.

One other note, to take advantage of colored syntax you'll need to make sure your terminal has colors enabled. To do that simply add the lines below to your bash profile.

    export CLICOLOR=1
    export LSCOLORS=GxFxCxDxBxegedabagaced

## An Honest Assessment
This post was not intended to convert, but more aptly stated as a viewport in to how I develop and why I love it. Many of the GUI editors on the market today have a lot of functionality and are very beautifuly done. I have only shown you a small glimpse of what Emacs can do, so I encourage you to go out and see for yourself the power that lies in Emacs and lisp. I leave you with a very apropos XKCD comic on the subject. [Real Programmers](http://xkcd.com/378/)