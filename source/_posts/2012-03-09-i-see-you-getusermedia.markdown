---
layout: post
title: "I see you getUserMedia"
date: 2012-03-09 22:43
comments: true
categories: [Writing Challenge, html5, javascript, peeping tom ]
author: Tony Camp
---

### Day 15 of the 30 Day Writing Challenge. Halfway there!

I'm busting my ass to get this post in. I hate just throwing "anything" up, so I took some time to research something I've never messed with: getUserMedia.

Let me preface this by giving you some requirements. Your computer needs a camera on it. You also need [Chrome Canary][1], or [Opera Next][0]. If you chose to use Opera Next, you don't have to enable any extra settings. If you decided to get Chrome Canary, you will have to enable one option. To do so, go to [chrome://flags/][2] and look for "Enable MediaStream". Make sure this is enabled. Now time for the fun.

{% include getUserMedia.html %}

<!-- more -->

If you have met all the requirements, you should see a live stream of your camera in the video player. Pretty fun right? And below the video stream, there's a "Take Picture" button. Click it! Makes sexy happen! Before we dive in and see how it was done, let me say there is a ton of good info on this out there on the webs. I was inspired by Rick Waldron, Mike Taylor and others when I started messing with this. I saw some shit Rick was posting on [Bocoup's site][6] and it blew me away. Also, [Mike Taylor's photo booth][3] is ridic. There's no way I could have gotten any of this done if I didn't take a peek at what they were doing. Now, let's check out some code.

{% gist 2010552 %}

Simple right? We just need an html5 video element, a button, and a canvas element. You can see in my comment, that you are able to drop in a default video incase the getUserMedia isn't available on the user's browser. In Mike Taylor's photobooth, if you don't have the getMediaUser method available, you can still "photobooth" a clip from a tasty Cary Grant video.

On to the guts, the JS, the fun part. I do have to let you in on a secet... I was rushed to get this out before midnight so it might be a tad sloppy. Deal with it!

{% gist 2010556 %}

I set up a lazy reference to document, fat finger, no make typey good. I give myself a shortcut. Next I just set up some variables. `snapshot` is my canvas element. This is where the photo we take will be placed. Now let's give the canvas some methods to use `.getContext()`. This comes in handy when we need to use the `.drawImage()` method. Next I set up a variable for the video element. And finally some functions. I will touch briefly on these.

```javascript
computeSize = function(supportsObjectFit){
		// user agents that don't support object-fit 
		// will display the video with a different 
		// aspect ratio. 
			if (supportsObjectFit == true){
				VIDEO_WIDTH = 640;
				VIDEO_HEIGHT = 480;
			} else {
				VIDEO_WIDTH = video.videoWidth;
				VIDEO_HEIGHT = video.videoHeight;
			}
		}
```
I know I'm calling this a bit wrong later in the code, but that's ok. This isn't production ready, for a client, or well groomed yet. It's a rough demo. But, let's break down the function. We are passing one argument, supportObjectFit. This comes from an Opera Specific CSS property called `object-fit`. It has to do with setting proper ratios of placed elements (video, canvas etc). [Read more about it here][5].

Next we have out success and fail callbacks for later:

```javascript
successCallback = function (stream) {
	video.src = stream;
	video.play();
	computeSize(true);
},
errorCallback =function (error) {
	console.error('An error occurred: [CODE ' + error.code + ']');
	computeSize(true);
}
```
These come into play a bit later when we're doing our feature detection on the getUserMedia method. On success, we're setting the src of our video element to be the "feed" from our device's video camera. We're also determining the size of the video element if the user is browsing with the Opera Next browser.

```javascript
doc.getElementById('take').onclick = function() {
	computeSize(true);
	photo.drawImage(video, 0, 0, VIDEO_WIDTH, VIDEO_HEIGHT);
};
```

Easy Peasy. Set up an click event on the "Take Picture" button. It will run the conputeSize() function, and then draw our screen shot from the video (aka our camera feed) onto our canvas element.

Now here comes the important part, everything else was basically for setting this up:

```javascript
window.addEventListener('DOMContentLoaded', function() {

	if (navigator.getUserMedia) {

		navigator.getUserMedia({'video': true}, successCallback, errorCallback);

	} else if (navigator.webkitGetUserMedia) {
		
		navigator.webkitGetUserMedia("video", function(stream){
			video.src = window.webkitURL.createObjectURL(stream);
			video.play();
			computeSize(false);
		});

	} else {
		console.log('Native web camera streaming (getUserMedia) is not supported in this browser.');
	}

}, false);
```
Here's where feature detection comes in to play. When the document is loaded, let's check to see if the browser supports our getUserMedia hawtness (currently, meaning the browser is Opera). If that is truthy, let's use it. In our example, we're just grabbing video by passing in the `{'video': true}` arguement, but you could also pass it audio as well. We also pass in 2 callbacks as arguements. The success and error functions we covered earlier. DONE!

Now we check to see if the user is browsing with the Chrome Canary build. The syntax changes a tiny bit but the idea is the same. Set the video element's source to be our camer feed, start the camera and all the computeSize() function. Since we know the user isn't in Opera Live here, we pass the argument "false".

If all else fails, log a msg showing this shit ain't gonna work.

I look forward to messing with this some more. I want to save the images users take and give them the ability to upload them to the TagSoup page. I will be working on that next. Please check out the links from Mike Taylor, Rick Waldron, Opera, and Google Devs.

11:56... Just made it.

[0]: http://www.opera.com/browser/next/
[1]: http://tools.google.com/dlpage/chromesxs
[2]: chrome://flags/
[3]: http://miketaylr.com/photobooth/
[4]: https://developer.mozilla.org/en/Canvas_tutorial/Using_images
[5]: http://dev.opera.com/articles/view/css3-object-fit-object-position/
[6]: http://bocoup.com/