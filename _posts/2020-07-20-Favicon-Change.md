---
layout: post
title: Changing the Favicon
---

Look up, now back at me. Now back up at the left of the current tab's title. Now back at me.

If you made it through the stale meme and noticed the Favicon is no longer the default one, congrats.

This change took forever.

I first made the favicon in GIMP. In order to get the SVG and PNG versions just right, it took me hours of "tweaking" (Read: Fighting with GIMP because I was very rusty)

![PNG version]({{ site.url }}/images/Sweat.png)

Then, I figured all I had to do was add a line of code to this blog's index.html like the tutorials stated... except it didn't work, and ended up breaking the site.

I reread the tutorial, and realized that the current Index.html doesn't actually have a "Head" section.

Poking around a bit more, I found a folder labeled "_includes" which had "head.html" in it.

Lo and behold, there was this line of code in it:

```
<link rel="shortcut icon" href="{{ site.baseurl }}/public/favicon.ico">
```

That line of code says "favicon". So all that needed to be done was to change it to refer to my favicon file and the location it was located at...

```
<link rel="shortcut icon" type="image/x-icon" href="{{ site.baseurl }}/images/favicon.ico">
```

I probably will have to make an "apple touch icon" later, but for now, I'm pretty happy.