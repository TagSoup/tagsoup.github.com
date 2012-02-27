---
layout: post
title: "I'll Show You Mine if You Show Me Yours"
date: 2012-02-26 22:06
comments: true
categories: [Writing Challenge, zsh, ohmyzsh]
author: Tony Camp 
---
###Day 3 of the 30 Day Challenge

Working in the default themed Terminal all day will make any sane (no such thing) developer cry. I'm sure we each have our own favorite customizations. Me? I'm a zsh sort of guy. Using it for a while (thanks [@slashclee][6]) I find myself switching up the themes each time I configure or reconfigure a computer. First thing's first though, I'm using a mac. I'm not a "fan boy", I'm just comfortable with it at this point. So if you're wanting to get this going on your Linux system, there are links that can help you... I'm sorry, but I can't help.

Enough words, how about some hawt pics?
{% img left /images/zsh.png 800 %}

####First things first: What are you staring at?
Well, that's my desktop, with some icons, a nice desktop pic and of course my terminal. I like the way it all looks. How come your Terminal doesn't look like that? Maybe I can help. If you're running OSX Lion and haven't already switched the default bash shell to zsh, you might want to do that now. Why switch? There are a ton of articles out on google explaning why people like it so much. For me, it comes down to a few basic benefits:
<ol>
	<li>Powerful tab completetion</li>
	<li>Styling</li>
	<li>Plugins</li>
	<li>Auto correct</li>
</ol>

###Switching to zsh
This part is easy, just go to your System Preferences and click on Users & Groups. Right click, or Control-Click on your username and open up the Advanced Options. If you're not able to Right-Click or Control-Click on your username, make sure the lock icon in the bottom left of the window is unlocked. Just click on it to toggle. Look for the dropdown for Login Shell and choose `/bin/zsh`. Fin

###Oh My ZSH
The next very handy tool that goes hand in hand with zsh is [Oh-My-Zsh][0]. Oh-My-Zsh really starts to open up the "Styling" and "Plugin" benefits I brought up earlier. Please, take the time to check out the repo and check out everything is has to offer. I'm not going to try to get into all of it on this post.

###Customizing Terminal
I'm using [Tomislav Filipčić's Solarized theme][2] for my Terminal. Grab that, unzip it and run the Solarized Dark.terminal file. This will create a new theme based on the ever popular [Solarized by Ethan Schoonover][3]. Select Solarized Dark under Terminal > Preferences > Settings.

{% img center /images/solarized.dark.png 500 'term bo' %}

###Customizing Oh-My-Zsh
Most of the customizations will be set in your .zshrc file. I'm assuming your Terminal is already open and hopefully you already your shell set to zsh. Type `~/.` and press `Tab`. Autocomplete, yah, no HUGE difference yet. Press `Tab` again. Yay, our first noticeable change. You can Tab-cycle through the files/dirs. Look for `.zshrc`.  This is your zsh profile. Look for the line that starts with `ZSH_THEME=`. This is where we are going to set our theme. [Check out some of the zsh themes.][1] I have mine set to blinks. But it's not the default blinks. I have done some tweaking on it. Since this section is about customizing, let's get to it. 
{% gist 1921784 zsh %}

There's a lot going on here. A lot. Without going too crazy, let's look at one small chunk to see what it's doing.
```
export PROMPT=$'%{\e[1;34m%}%n@\e[1;34m%M%{\e[0m%}
%{\e[0;%(?.32.31)m%}>%{\e[0m%} '

export RPROMPT=$'%{\e[1;31m%}$(project_pwd)%{\e[0;34m%}$(ruby_version)$(git_cwd_info)%{\e[0m%}'
```
So zsh let's have have a "left" and "right" side of the prompt. On the left, I like it nice and plain. So we're outputting the username@system-name. Then we have a line break where our prompt will be. And on the right hand side of the screen, we have even more useful info. We're displaying the current directory, the version of Ruby, the current git branch and SHA. I have added some fun colors to it, cause you know, colors are good mmmkay.

###On to plugins
The plugins in zsh are really useful. They will extend autocompletion for you and set up some aliases for you. Back in your `~/.zshrc` file look for the line that starts with:
```
# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Example format: plugins=(rails git textmate ruby lighthouse)
```

The plugins I have installed are git, github, node, gem, rails, ant. It looks like this:
```
# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Example format: plugins=(rails git textmate ruby lighthouse)
plugins=(git github node npm gem rails ant)
```
Most of these I don't use to their fullest. But you can [take a look at some of the things the git plugin is doing for us.][4] If you're using git a lot, some of the aliases will save you some time. [Check out all of the available plugins for zsh.][5]

I'm by no means a shell, or zsh expert. I'm still learning a ton as I go. The day I spent getting Solarized set up and the prompt on the right caused big head aches for me. I have a whole list of git aliases, and standard aliases to share that will tie in nicely with this post. This post has already gotten much longer than I like. TL:DR. If you have any question, please feel free to ask. I will do my best in answering or finding an answer. Use the comments below or msg me on github.

###Hepful Links
[Oh My ZSH github repo][0]<br />
[ZSH Homepage][7] - A little overwhelming<br />
[Steve's Extravagant Zsh Prompt][8]<br />
[Oh My ZSH Twitter][9] - Blue birds<br />

<div class="clearfix">
{% img left /images/whip-it.jpg 60 %}
{% img left /images/whip-it.jpg 60 %}
{% img left /images/whip-it.jpg 60 %}
{% img left /images/whip-it.jpg 60 %}
{% img left /images/whip-it.jpg 60 %}
{% img left /images/whip-it.jpg 60 %}
</div>

[0]: https://github.com/robbyrussell/oh-my-zsh
[1]: https://github.com/robbyrussell/oh-my-zsh/wiki/themes
[2]: https://github.com/tomislav/osx-lion-terminal.app-colors-solarized
[3]: http://ethanschoonover.com/solarized
[4]: https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh
[5]: https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins
[6]: https://twitter.com/#!/slashclee
[7]: http://www.zsh.org/
[8]: http://stevelosh.com/blog/2010/02/my-extravagant-zsh-prompt/
[9]: https://twitter.com/#!/ohmyzsh