# How I built a social netork like Instagram in a few lines of code using flutter and why i fell in love with it
## What I wanted to build and why I tried Flutter
As of last year I had very fun building my own little social network called Lime. So far you can post images and text messages  to Lime, give posts a "Like" and comment/chat on them. The clue: All messages are tighly bound to the location it was posted. So you are only able to see posts which have been posted in an at maximum 30km radius. Here is what it looks like:

![](https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-preview.gif?raw=true)


<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-preview.png?raw=true" width="500">

But the App had one big problem: I didnt develop an iOS Version! So because I do not own an Apple Computer nor do I own an iPhone I decided to look around for any cool Framework which would allow me to develop an App for both platforms. This way I would be able to build and debug it on my linux machine and my Android smartphone, which sounded great to me. 
Since I am a great fan of Dart as a programming language, I found Flutter. And hell: I remember seeing the first code example of a layout and immediatelly leaving the website shaking my head. So I visited all the other well known Frameworks like Xamarin, ReactNative or NativeScript and those are all great Frameworks but nothing could catch me entirly. So I cloned Flutter, gave it a chance by reading the docs and tried building my first layouts. The reason: I like Dart and I like IntelliJ and I love the idea that Flutter runs inside a VM and draws everything by its own. Since then I needed 10 hours of work (reading the docs included) to built the following and here is how I did it. 
