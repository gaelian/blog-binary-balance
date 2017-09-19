---
title: Going on Safari
tags: [browsers, safari, chrome, user-interface, open-web]
---
A little while back I read [a post by DHH](https://m.signalvnoise.com/celebrate-the-web-by-using-another-browser-than-googles-chrome-174a45991c42) wherein he lays out his reasons for why we should be more omnivorous with our browser use, lest we end up back in a new version of the bad old days when [Internet Explorer](http://knowyourmeme.com/memes/subcultures/internet-explorer) was the only show in town, holding back the progress of the open web. [For some time now](/2011/10/03/it-is-time-to-chrome-my-web-browsing) and like seemingly most people these days, my main browser has been Google's Chrome but David's post struck a chord with me and so - being on MacOS - the obvious alternative to Chrome that I could use is Safari. This is my experience of a long time Chrome user giving Safari an earnest go.<!--more-->

### Work vs Play

So like David, I would add that I still use Chrome for web development work. but it's not for lack of trying with Safari. As one example: lately, a lot of the hands-on web development I have done involves some fairly intensive work with Microsoft's [Dynamics 365](https://en.wikipedia.org/wiki/Dynamics_365) platform and I've noticed that D365 just does not seem to get along as well with Safari as it does with Chrome. From time to time I see cryptic and seemingly random JavaScript errors popping up in Safari that I don't see in Chrome (quite possibly D365's fault more so than Safari, not sure). D365 is a fairly JavaScript heavy web application and the general feeling I get while using Safari with D365 is that Safari just isn't processing client-side code as quickly as Chrome does. Apple boasts that [Safari is faster at processing JavaScript than other modern browsers](https://www.apple.com/au/safari/)... maybe it's just the sites and web applications I tend to deal with, but it doesn't typically feel this way to me.

While at one time sharing ancestry via aspects of [WebKit](https://en.wikipedia.org/wiki/WebKit), Chrome and Safari now seem to have diverged enough that there are some occasional rendering differences I encounter across the two browsers. For me this has been in scenarios with complex CSS where many layered styles make it hard to easily determine which combination of styles might be causing said differences.

As has always been the case, if one is using some of the more avant-garde CSS3 features, then some differences in browser support is to be expected. But still, what web developers need to deal with these days in terms of cross-browser incompatibilities is a dream compared to 10-15 years ago.

I personally find the developer tools available in Chrome more usable but this may well just be that I've spent more time with Chrome for dev work.

My overall point here is that Chrome to me is still a better and generally easier to use developer's browser. Although Safari has come a long way since the last time I spent any reasonable amount of time with it.

### Smart Search field

On my high spec 2016 MacBook Pro, coupled with an relatively fast Internet connection, searches using Safari's "Smart Search" field are randomly slow at times. Sometimes the search will happen more or less instantly, other times Safari will seemingly sit there for maybe five or six seconds showing no activity at all before returning the Google search results page. Doing the equivalent in Chrome, I don't think I ever get a similar delay. Unticking various features in Safari's preferences under the "Search" tab can apparently make some difference and speed things up a bit, I don't think it has for me. My laptop is still pretty new with fully up to date software, my Internet connection is pretty fast (in Australian terms). The way I'm using Safari's search is not unusual and should be catered for by the default search settings without causing weird intermittent delays. Smells like a bug to me.

### Mousing over links to show the URL

I didn't even realise this was important to me until I started using Safari and realised this feature was gone. If you mouse over a link in Chrome, you will see the URL near the bottom of the browser window. Safari does not do this by default and I found it surprisingly annoying. Call me paranoid, but in this day and age of [phishing scams](https://en.wikipedia.org/wiki/Phishing) and [NSFW links](https://en.wikipedia.org/wiki/Not_safe_for_work) (is this actually a NSFW link? How might you know before you click it? :P), I often like to be able to see at a glance the URL of a link before I visit that URL. Because I suspect I probably am in the minority on this use case, I guess it's fair enough that I have to [change a setting to make this happen in Safari](https://apple.stackexchange.com/questions/16759/how-can-i-make-safari-show-the-url-when-i-hover-over-a-link).

### Safari browser extensions

The selection of browser extensions available for Safari is quite limited. luckily I've become pretty conservative in regards to which browser extensions I install[^1] so I have only a small handful of browser extensions that I really need now anyway, the important ones of which I have found for Safari or some acceptable equivalent thereof. I have noticed however that some of the Safari extensions I do use don't seem to work quite as well as their Chrome counterparts. Whether the problem is more on the Safari side or the extension side, I'm not currently sure.

### Battery life

Apple say that the have optimised Safari running on Mac hardware and MacOS to maximise battery life and this really shows in my experience (Apple claims 2 hours longer browsing than Chrome/Firefox, 4 hours longer Netflix watching than Chrome/Firefox). I have noticed that Chrome devours my laptop battery while Safari does indeed seem to only sip it like a fine wine. The only other thing I use regularly that eats my battery faster than Chrome is running a full Windows virtual machine (try both at the same time for bonus battery death!). Generally any battery conservation I can get is a pretty killer feature for me when I'm out and about.

### Nice integration with the OS

As with most of Apple's own applications, Safari has some nice little points of integration between it and the Touch Bar on my MacBook Pro, showing little thumbnail images of pages I have open in Safari and allowing me to tap the thumbnails to switch between them, also present is a button on the Touch Bar that opens the (intermittently slow) "Smart Search" and allows access to favourites. Not really "must have" features, more "nice to have". I think that last sentence generally sums up the Touch Bar on MacBook Pros actually (not so for the fingerprint reader which comes along with the Touch Bar though, love the fingerprint reader!) but enough has probably been written on this already elsewhere.

### TL;DR

For everyday browsing and if you have a Mac, then I would recommend Safari. It's a decent browser these days and the work Apple has done to integrate it into MacOS and maximise battery life while browsing with Safari really shows.

You don't have to go on Safari, but do consider trying out a browser other than Chrome and do your little part to support the open web.

[^1]: The recent [compromise of Web Developer for Chrome](https://twitter.com/chrispederick/status/892768218162487300?lang=en) was bit of a wake-up call for me. If you're a developer, then you probably already know that browser extensions can run with very privileged access on your system and can have direct access to all manner of sensitive personal details (you use online banking right?). But this incident drove it home for me. You don't even need to install an outright malicious extension, even competent and upstanding developers like Chris Pederick can still make mistakes, all of us can. These days I have to be very sure that I need a given browser extension and will carefully consider which permissions the extension needs before installing it.
