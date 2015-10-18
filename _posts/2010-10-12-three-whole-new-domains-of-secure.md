---
title: Three whole new domains of secure
enki_id: 7
tags: [ecommerce, fail]
---
Last week my partner asked me to buy her some printer ink cartridges at a particular online store that sells such things so armed with her Visa card I undertook the quest.<!--more-->

I found the desired ink cartridges and proceeded to check out. I arrive at a page that tells me it has been detected that I am a part of the ‚'Verified by Visa' program and that I will be sent to a page to facilitate said verification by Visa. I'm redirected to a page that by the URL and branding I assume is owned by the bank that provides the merchant facilities for the ink cartridge website. This page partially loads but then dies because my [NoScript Firefox Add-On](https://addons.mozilla.org/en-US/firefox/addon/722/) disallows the JavaScript on the page. I'm left looking at a button that says something like 'Click to enter your password' but due to JavaScript being disabled, it does nothing when you click on it and even if it did, I would not know what password to enter. I do some Googling on Verified by Visa and how it might relate to our particular bank, but find very little other than pages filled with sales-speak on how Verified by Visa makes things lots more secure.

Conversation ensues:

Me: 'Do you have a password for your Visa card?'

Partner: 'Password? I have a pin number...'

Me: 'No I think it's different to your pin number.'

Partner: 'I have no idea what you're talking about.'

The temperature starts to rise as it's late and it's been a long day for both of us. The conversation escalates but is defused just in time before a nuclear strike is called in by either side.

Back to the Interwebs, I enable JavaScript, the page then reloads and errors out because now it's been detected that I've done something out of the ordinary flow and it's all bets off. I get back to the original ink cartridge website to find my shopping cart empty.

Selecting all the products I wish to purchase for a second time and not realising at this stage what Verified by Visa actually is and my tired and befuddled mind already convinced that the bank has enrolled my partner in some added security scheme and that she's forgotten that they gave her a password, I try using my own MasterCard and find out that I hit the same thing, only with MasterCard it's called MasterCard SecureCode. Fortunately since I enabled JavaScript last time, I get through and interestingly without having to enter any password, even though the explanatory text still told me I would need to.

The next day, my partner goes to the bank, the person behind the counter has no idea what Verified by Visa or MasterCard SecureCode actually is. They tell her they do not supply passwords for credit cards. My partner then calls me and asks me to explain what happened to the person at the bank, more frustration. My partner eventually finds someone at the bank who knows vaguely what she's talking about and he tells her that they don't supply passwords for credit cards, but that you should be able to set one up at some point during the checkout process on this random, not particularly trusted website that she buys printer cartridges from.

So. Much. Win.

### WTF just happened?

After doing some more research, it turns out that this is in fact some kind of partially aborted implementation of a what's known as [3-D Secure](http://en.wikipedia.org/wiki/3-D_Secure) an added layer of security used by Visa and MasterCard under the names Verified by Visa and MasterCard SecureCode respectively.

I'm going to have to put some of the blame for this whole amusing (in hindsight) cluster fuck onto myself because I'm meant to know about this stuff, I'm paranoid enough to be using NoScript and the night in question was not my most blindingly brilliant moment of deductive reasoning. But none the less, this was still without a doubt the worst sales experience that I have had in all the years I have been buying stuff off the Internet. Regardless of the merit of 3-D Secure it's self, if this is the current state of its implementation then yeah, no thanks.

3-D Secure? More like... *3-D Manure!* Sorry, couldn't resist.
