---
title: Is it time to chrome my web browsing?
enki_id: 11
tags: [ruby, rails, google, firefox, chrome, browsers, javascript]
---
I remember when I first heard of Google's new browser, dubbed [Chrome](http://www.google.com/chrome). I was sitting at my desk at work, reading the news and for a moment I thought I could almost hear the wailing and gnashing of teeth emanating from clear across the other side of the world at Redmond.<!--more-->

There were some obvious wins for Chrome right out of the gate. Independent processes for tabs meant that if a page crashed it wouldn't take down the whole browser with it. Security was also enhanced with this model of tab isolation, if I remember correctly. There was the [V8 JavaScript engine](http://en.wikipedia.org/wiki/V8_%28JavaScript_engine%29), tests by Google in September 2008 showed that JavaScript execution in Chrome was about three times faster than [Firefox](http://www.mozilla-europe.org/en/firefox/) 3.0 and about ten times faster than Internet Explorer 7. There was some debate over the real world applicability of these results, but little doubt that Chrome was kicking ass at JavaScript. A modern browser made to support the quickly burgeoning age of JavaScript heavy web applications.

But there was also the question of [privacy](http://coderrr.wordpress.com/2008/09/03/google-chrome-privacy-worse-than-you-think/) while using Chrome. Google responded by saying that they would quickly render anonymous the data collected from people's use of Chrome. There was some [criticism of this also](http://news.cnet.com/8301-13739_3-10038963-46.html?tag=mncol;title). Looking at [Chrome's privacy policy](http://www.google.com/chrome/intl/en/privacy.html) today, it doesn't seem like much has changed. Although it would appear that you can disable most if not all of the possibly concerning features in Chrome. I'm consciously being [less paranoid about my online privacy](/2010/08/25/online-privacy-inspect) these days and let's be honest, you'd have to be a pretty atypical Internet user for Google to not have a million little clues to who you are and what you've been doing online. So – unless you've managed to abstain from all things Google related over the past 12 years or so – maybe it's time to just build a bridge.

Beyond the pros and cons of Chrome, I had been a Firefox man for a long time by then and while I immediately had to download and try out this new browser, I knew that I wouldn't be switching full time to Chrome at least until it had the same add-on support and evolved add-on ecosystem that I enjoyed with Firefox. Chrome has had the ability to support add-ons for some time now and I haven't really had the time or the inclination to investigate how the current crop of Chrome extensions and features stack up against Firefox for my purposes. That is, until now.

Looking at my Firefox add-ons currently, I've got:

- 1Password extension for Firefox
- Adblock Plus
- All-in-One Gestures
- Backpage Pages
- Change
- ColorZilla
- Dictionary Search
- DOM Inspector
- English (Australian) Dictionary
- Firebug
- Google Reader Watcher
- Image Zoom
- Live HTTP Headers
- MeasureIt
- NoScript
- Session Maanger
- Tab Mix Plus
- Web Developer

That's a fair list, though I'm guessing not as full as some people's Firefox installs. There's probably at least a few add-ons in that list I could get rid of and not really notice any difference. But some I can no longer live without.

### Web development add-ons

There's an unsurprising theme evident: many of these add-ons are related to web design and development. ColorZilla, DOM Inspector, Firebug, Live HTTP Headers, MeasureIt and the Web Developer toolbar would all fall into this category for me. Out of these tools, Web Developer and Firebug I cannot do without.

#### Web Developer (toolbar)

Whether it's having two easy clicks to disable JavaScript while I test something, resize the browser window to an exact height/width to see how a page behaves at a certain viewport size, or selectively disable stylesheets, [Web Developer](https://addons.mozilla.org/en-US/firefox/addon/60/) is there to make my life easier. Having first installed this add-on years ago now, there's still a whole bunch of features in it that I've never used, but looking through the drop-down menus, I feel comforted just knowing they're there.

A version of Web Developer has been created for Chrome, but it's lacking some stuff that I would miss. [Like disabling JavaScript and the cache](http://chrispederick.com/work/web-developer/help/).

#### Firebug

How did people write JavaScript before [Firebug](https://addons.mozilla.org/en-US/firefox/addon/1843/)? I'm having trouble remembering but I'm pretty sure it was like pulling a heavy wooden cart with square wheels filled with screaming monkeys flinging their faeces at the back of your head[^1].

There is no direct corollary for Firebug on Chrome, though I did find [Firebug Lite](http://getfirebug.com/releases/lite/chrome/). And Chrome's built in Developer Tools are a good alternative that [seems to have won some converts](http://blog.stormid.com/2009/06/chrome-developer-tools-vs-firefox.html).

### General purpose add-ons

Of the add-ons not directly related to web design/development, probably the five I use the most are 1Password extension for Firefox, All-in-One Gestures, Backpack Pages, Dictionary Search and Google Reader Watcher.

#### 1Password extension for Firefox

An extension that comes with and hooks into [1Password](http://agilewebsolutions.com/onepassword), my password management software of choice, there has just recently been released a version of [this extension that supports Chrome](http://help.agile.ws/1Password3/google_chrome_support.html).

#### All-in-One Gestures

Mouse gestures are awesome. When a friend first recommended [All-in-One Gestures](https://addons.mozilla.org/firefox/addon/12), I remember thinking this was like using the mouse gestures in Peter Molyneux's ambitious game [Black and White](http://en.wikipedia.org/wiki/Black_and_White_%28game%29) to cast miracles, but in your browser! I don't even often use the full plethora of gestures available. Probably 99% of the time I'm just using back/forward (draw a line right to left/left to right) and drawing a line up over a link to open in a new tab. But I've gotten so used to just these three simple gestures that I'm always momentarily confused when I go for a gesture and happen to be working at a foreign computer, using a browser that does not have a mouse gestures add-on installed.

Does Chrome have mouse gestures? Yes it does. But apparently [not yet for Linux or Mac](http://code.google.com/p/chromium/issues/detail?id=28226). That dog don't hunt.

#### Backpack Pages

[Backpack Pages](https://addons.mozilla.org/firefox/addon/1544) is a Firefox add-on that lets you access your [Backpack](http://backpackit.com/) from an icon on the right hand side of the Bookmark Toolbar in Firefox. I've been a Backpack user since 2005 when I read an [article on Salon.com](http://www.salon.com/technology/feature/2005/08/10/37signals/print.html). I wanted to see what this [Ruby on Rails](http://rubyonrails.org/) stuff was producing and I liked what I saw.

I've found [one extension for Chrome](http://productblog.37signals.com/products/2010/02/google-chrome-extension-lets-you-create-new-reminders-in-backpack.html) that allows manipulation of Backpack reminders, but I'm looking for a bit more than that.

#### Dictionary Search

There is not surprisingly at least two [perfectly](https://chrome.google.com/extensions/detail/mgijmajocgfcbeboacabfgobmjgjcoja?hl=en) [good](https://chrome.google.com/extensions/detail/ipdjaafajlfiopcppipdinmcjbcpofhd) Chrome equivalents for [Dictionary Search](https://addons.mozilla.org/en-US/firefox/addon/68/).

#### Google Reader Watcher

Google Reader being a Google product, I'd again be surprised if there wasn't an equivalent for the Firefox [Google Reader Watcher](https://addons.mozilla.org/en-US/firefox/addon/4808/) add-on. Enter [Google Reader Notifier](https://chrome.google.com/extensions/detail/apflmjolhbonpkbkooiamcnenbmbjcbf).

### Conclusion

I'll be sticking with Firefox as my primary browser for now. But does it have to be an either/or? Not really. I have heard of people who might use Chrome or Firefox as their development browser and switch to say, Safari for general browsing because they just like the cleaner interface. Or maybe Chrome for the JavaScript performance.

However, I seem to be something of a browser monogamist. Part of this I'm sure is just habit. But part of it is because for me there's often a pretty blurred line between development browsing and general browsing. Often I'll be surfing along, come to a site, see something cool and think 'I wonder how they did that.' At those times I like to be able to have my arsenal of web development add-ons at the ready to delve into the site and see what I can find.

[^1]: It felt kind of like this before Prototype and jQuery too, but the monkeys had Rabies.
