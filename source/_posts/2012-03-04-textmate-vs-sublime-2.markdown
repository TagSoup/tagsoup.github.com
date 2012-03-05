---
layout: post
title: "Textmate vs Sublime 2"
date: 2012-03-04 21:03
comments: true
categories: [Writing Challenge, Text Editors, Tools]
author: Tony Camp
---

###Day 10 of the 30 Day Writing Challenge

I love, and I mean LOVE TextMate. It was my baby up until this year. TextMate did some amazing things, but their lack of updates started to wear on me. I had seen a few friends starting to use Sublime Text 2 and, at first, totally ignored their praise of the product. I thought, there's NOTHING Sublime is doing that TextMate (TM) isn't. So, it took about a month from the time I saw Sublime, to the time I gave it a try. I wish I would have done it earlier. I've seen the screen shots of TM2 and can't form a REAL opinion of it without using it. So the cries of "TextMate 2 is going to be better" mean nothing to me right now. I have gone through, and compared the tool based on what I am doing day to day. I'm purely a front-ender. And my day to day, may be completely different than yours. Just roll with it.

<!--more-->

#### Works on all platforms
TextMate: OSX Only<br />
Sublime: OSX, Linux, Windows<br />

Clear choice there. Your whole team can be using the same editor, no matter which platform they're on. Zesty.

#### Selecting Lines of Code 
TextMate: `cmd-shift-L`<br />
Sublime: `cmd-L`<br />

I don't think I need to explain what this keystroke is doing. The reason TM needs the `shift` modifier is because cmd-L performs a "Go to line #" command. 'nuff said

#### Moving lines of code up and down
Sublime and TextMate: `cmd+ctrl` up and down arrows

There are some subtle differences in the two though, so read on.

TextMate: When you have an entire line selected (cmd-L), it will move the line up or down one line at a time. You can continue to move the selected line up or down multiple lines by repeating the command. BUT, if you have your cursor on the line of code, but it's not selected the line of code moves and the cursor stays put. So if you perform the keystroke again, you will be moving a different line of code. The rule is: the line the cursor is sitting at is the line that will be moved.

Sublime: Performs the same when the entire line is selected and when the cursor is on a line of code. When the keystroke is performed, the lines moves in accordance to the arrow key pressed, but the cursor also move with that line. So even if you don't have the entire line selected, you can perform the keystroke repeatedly and move the same line up or down your code.

#### Wrapping the guts
TextMate: `cmd-shift-w`<br />
Sublime: `cmd-shift-w`<br />

Both applications do the same thing. It starts an html tag by both opening and closing the tag. Both default to the following:
```
<p></p>
```
The nice part is the cursor is placed in the first opening tag, with the 'p' highlighted. So if you want to change the `<p>` to a `<div>` just type away.

#### Wrap multiple lines:
TextMate: `cmd-ctrl-w`<br />
Sublime: Can no findz<br />

TextMate: This is a handy shortcut for ul, ol, dl, and omgl too. Here's how it goes down, stick with me. You start from the inside out. So think, the text of each list item. Like so:
```
OMG
This is
a mutha
effing list
```
Use select each of those lines, in TextMate I put the cursor on the first line and `cmd-shift-L` to select the line. Then hold `shift` and the down arrow to select the lines under the first. With me? Then I use the `cmd-ctrl-w` to wrap each line individually. I change the default `<p>` to `<a>` which makes:
```html
<a href="">OMG</a>
<a href="">This is</a>
<a href="">a mutha</a>
<a href="">effing list</a>
```
Sweet, so now I have a bunch of links, perhaps a nav or some sort. Now, I select all of the lines again, and perform the same keystroke which gives us:
```html
<li><a href="">OMG</a></li>
<li><a href="">This is</a></li>
<li><a href="">a mutha</a></li>
<li><a href="">effing list</a></li>
```
Then I select all of those lines again, and use `ctrl-shift-w` to wrap the list items in an `<ul>`. HAWT
```html
<ul>
	<li><a href="">OMG</a></li>
	<li><a href="">This is</a></li>
	<li><a href="">a mutha</a></li>
	<li><a href="">effing list</a></li>
</ul>
```
#### Multiple Cursors
TextMate: meh, kinda... `cmd-opt-a`<br />
Sublime: `cmd-left click`<br />

TextMate: Let's take the above example, the unordered list. You can select all of the `<li>` tags and press `cmd-opt-a` which will place a cursor on each line (invisible). It might not seem different from the column selecting but here's how it is. On lines of various lengths, the characters won't match up correctly. So what this command does, is place the cursor at the end of the line, and allows you to be X amount of characters from the end. So in the above example, I use my keystroke and press the `left arrows` 5 times and my cursor will be before each of the `</li>` tags. The nice thing is, it doesn't matter how long each line is. The drawback? The feature only works on line in succession. You can't skip lines because there's no Multi Select in TM

Sublime: Just `cmd-left click` anywhere you want. Yum


#### Switching File Types
TextMate: `ctrl-opt-cmd-"modyifier"`<br />
Sublime2: Can no finz<br />

TextMate: When you create a new file, by default, the new file type is a plain text document. You can use `ctrl-opt-cmd-h` to change it to html, giving  you html syntax highlighing. Or `ctrl-opt-cmd-j` which will prompt you with a choice, Java or JavaScript. That one's easy, JS! A handy short cut for sure. Wish Sublime had this.

#### Bundles
TextMate: Yups<br />
Sublime: Yups<br />

Even better, Sublime can use most TextMate bundles. DO IT!

#### Multi-Select
TextMate: No haz :(<br />
Sublime: `cmd` and select text<br />

Mmmmm, so delicious. So you can select multiple strings, tags, etc, from ANYWHERE in the document. Not just columns. Plese see this quick, [blurry demo][0].

#### Package Control
TextMate: No haz :(<br />
Sublime: `cmd-shift-p`<br />

Add new plugins with a few keystrokes. Type that short command, then in the prompt begin typing "Package" and look for "Package Control- Install Package". Take a look at what' available.

#### Jump to line number
TextMate: `cmd-L`<br />
Sublime: `cmd-g`<br />

Um, so if I need to explain this, then I've wasted all my time writing this tonight. Frownies.

#### Multi-col selecting
TextMate: `alt-drag` (or however you wish to select)<br />
Sublime: `alt-drag` (or however you wish to select)<br />

This allows you to select multiple lines of text, and multiple colums on each line.

#### Tabbing multiple lines
TextMate: `cmd-[`<br />
Sublime: `tab`<br />

Again, I'm hoping you know what this does. 

#### JS Obj-like preferences
TextMate: Naws<br />
Sublime: Has<br />

TextMate allows for a good amount of customization in various menus etc. Sublime does it a bit different. Here' take a look for youself:
```javascript
{
	"color_scheme": "Packages/User/Monokai Soda.tmTheme",
	"detect_indentation": true,
	"draw_minimap_border": true,
	"find_selected_text": true,
	"font_size": 12.0,
	"highlight_active_indent_guide": true,
	"highlight_line": true,
	"indent_subsequent_lines": true,
	"tab_completion": true,
	"theme": "Soda Dark.sublime-theme",
	"trim_trailing_white_space_on_save": false
}
```
This makes it VERY easy to change computers without losing your customizations. Upload this to a gist, and you're set.

#### Tab Completion
TextMate: naws<br />
Sublime: Yups<br />

Sublime offers tab completion for multiple languages. And it's damn good.

#### UI
TextMate: Loses, needs 3rd party stuffs to compete<br />
Sublime: Winsies<br />

I know text editors are like religions. So I am scared to get TOO deep into the discussion of why Sublime has a nicer UI.

<ul>
	<li>The zoomed-out look of the document on the right hand side is a win with Sublime.</li>
	<li>The default drawers (think furniture not art). Clicking on a file in the drawer doesn't open it, but gives you a look at what's in the file. As soon as you start editing, it snaps open. I remember sometimes having shit-tons of documents open in TextMate because I was clicking on files trying to find something specific (yes, I know about find).
	</li>
	<li>Distraction Free Mode!!!!</li>
</ul>

#### TL:DR;
Trust me when I say, making the switch was a hard sell. In fact, I left my TextMate icon in my dock for 3 months after making the switch. I was sure there was going to be something in Sublime that was going to piss me off. I'm still using Sublime and I'm not looking back. Even when/if TextMate 2 ever releases.

#### Useful Links Getting Started with Sublime 2
[NetTuts Getting Started][1]<br />
[o2js Getting Started][2]<br />
[9 Reasons You Must Install Sublime][3]<br />
[More on Package Control][4]<br />
[Some Extra Syntax Highlighting][5]<br />

[0]:http://tagsoup.github.com/blog/2011/12/08/Sublime-2/
[1]:http://net.tutsplus.com/tutorials/tools-and-tips/sublime-text-2-tips-and-tricks/
[2]:http://o2js.com/2011/10/29/fell-in-love-with-sublime-text-2/
[3]:http://1p1e1.tumblr.com/post/14262857223/9-reasons-you-must-install-sublime-text-2-code-like-a
[4]:http://wbond.net/sublime_packages/package_control
[5]:https://github.com/n00ge/sublime-text-haml-sass