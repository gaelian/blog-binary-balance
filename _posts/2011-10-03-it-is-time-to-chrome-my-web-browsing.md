---
title: It is time to Chrome my web browsing
enki_id: 21
tags: [google, firefox, chrome, browsers, javascript]
---
Late last year, I wrote a post entitled ['Is it time to chrome my web browsing?']({% post_url 2010-12-01-is-it-time-to-chrome-my-web-browsing %}) wherein I took a look at how [Chrome](http://www.google.com/chrome) stacked up in comparison to [Firefox](http://www.mozilla.org/firefox/), specifically in relation to the Firefox Add-ons that I used and could not do without. At that time, I decided that - for me - the day was not yet ripe for a move.<!--more-->

Fast forward to ten months later and the situation has changed. For the last four months or so, I've been trialling Chrome as my default web browser for both day-to-day browsing and development. Enough time has passed now that I can say with some certainty that I won't be going back. And it seems that I am not alone as [Chrome is set to overtake Firefox](http://www.computerworld.com/s/article/9220396/Chrome_poised_to_take_No._2_browser_spot_from_Firefox) as the second most popular browser before the end of this year.

I will always have a soft spot in my heart for Firefox, the browser that saved us from the complacent evil that is [Internet Explorer 6](http://www.ie6countdown.com/), but Chrome's marked improvement in speed, stability and general streamlined-ness over recent releases of Firefox are just too clear to ignore. When I open Firefox now, I'm struck by how... ugly it looks. Cluttered and kind of crufty[^1]. Certainly on OS X, Firefox doesn't support the clean Apple aesthetic as well as does the OS X implementation of Chrome, but something similar could also be said for these browsers on Windows 7. In any case though, I am a developer and beyond most aesthetic concerns are concerns of functionality. The below Chrome extensions are what I have ended up with in order to more or less (mostly more) emulate the functionality I used to have with Firefox:

- Session Buddy
- Web Developer (toobar)
- Pinboard
- 1Password Beta
- AdBlock
- Currency Converter
- Firebug Lite for Google Chrome
- Google Reader Notifier
- Google Dictionary
- Smooth Gestures/Smooth Gestures: plugin

Largely, it's the development aids that are most important to me. With the development work I've done over the last few months, even though I've got Firebug Lite for Google Chrome installed, I actually haven't had to use it. The built in Chrome Developer Tools have been enough for my needs. The only thing I miss a little is the ease with which one can disable JavaScript using the Firefox version of the Web Developer toolbar, but I gather as soon as this is possible with the Chrome extension API, it will be implemented. The 1Password Chrome extension (still in beta) is nicely designed, although it has the one small annoyance of not being able to auto-fill login details when presented with basic HTTP authentication, you have to manually type/paste the details in yourself. As opposed to the Firefox 1Password Add-on which automatically fills in login details for basic HTTP authentication. There's only one semi-regular occasion where I need to login through basic HTTP authentication though, so this is no deal breaker. There is one other small Firefox Add-on that I miss, but more about this in my next post.

I really like the overall way that the Chrome extension API has been constructed. If you know how to build a web page with HTML/CSS/JS, then you already pretty much know how to build a Chrome extension, the barrier to entry is low for a web developer. The extension API also encourages a more unified and straight forward approach across both the user interface of Chrome and it's extensions that simply isn't possible with Firefox. Though I wonder if perhaps a little more suggested or enforced design convention from Google - for example when it comes to the options pages for Chrome extensions - might have been a good look. But on the other hand, maybe this wouldn't have been realistic, considering the large variations in functionality between different Chrome extensions.

I still find Firefox's 'AwesomeBar' a little more useful than Chrome's 'OmniBar'. The 'as you type' suggestions from the 'AwesomeBar' just seem a little more relevant. But this is not enough to outweigh the other advantages I get from Chrome. The 'Chrome Instant' feature seems like a good innovation[^2] in the majority of use cases, but I tried it out and quickly disabled it again after finding it annoying when I was working on a local development site and having my browser prematurely load local pages that I didn't want to go to.

So bearing in mind that this post is from a guy who had used Firefox for many years and was really looking for any excuse not to make the switch, is it time to Chrome my web browsing? Yes, yes it is.

[^1]: This is my (only slightly) customised install of Firefox I'm talking about and I am of course aware that buttons can be removed from the user interface and so on. My point is more that my installation of Firefox never looked cluttered or crufty before I got used to Chrome and subsequently, neither do I feel like I've lost any truly relevant functionality.

[^2]: [Privacy issues](http://www.google.com/support/chrome/bin/answer.py?answer=180655&hl=en) notwithstanding. I personally don't have much of a problem with the possibility that partial search queries will be sent to the search engine I'm using as well as my full search queries which they would be getting regardless.
