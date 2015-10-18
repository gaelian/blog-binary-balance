---
title: A wakeup call from online security, will you accept the charges?
enki_id: 27
tags: [google, security]
---
A couple of weeks ago, I got a call from my [ISP](http://en.wikipedia.org/wiki/Internet_service_provider) to tell me the email account attached to my ISP plan had been broken into and was being used to send spam. They understandably were forced to change the password on my account and called to let me know what had happened. This marks the first time in my relatively long history of Internet use that I've had an account broken into and it was something of a wakeup call for me.<!--more-->

The ISP support person asked if I'd clicked on any questionable links in emails received at my account recently. I was pleased to be able to unequivocally rule out this attack vector, as in fact I hadn't used that email address at all for at least a few years, it had long ago been relegated to function only as a forwarding address to my primary email account. The support person and I agreed that most likely, the account was cracked via a [brute-force attack](http://en.wikipedia.org/wiki/Brute-force_attack) on my ISP's web mail interface. While it wouldn't be unreasonable to question why their web mail login page is probably not hardened against brute force cracking, if indeed this was how the account was compromised I must also take some responsibility for what happened, as the password attached to my account was not as strong as it could have been. It was something of a legacy password from a bygone era when I wasn't as cognisant of online security, I had been meaning to change it for some time and just hadn't got around to it.

While I couldn't really care less about the email account that I don't use (I ended up asking my ISP to just disable it), of greater concern to me was the fact that the same password used to access my ISP email account was also the one that I use to access my Internet connection its self. I had no choice about this, its just the way their system works. Fortunately I don't think the attackers realised this before their access to my account was revoked. So the end result was that – aside from a little spam being sent out from my old, unused email address – there was no harm done.

However, things could have been worse and this situation prompted me to finish off ramping up the security on all my online accounts. The Internet is not as friendly a place as it was even a few years ago and it's long past the time when we should all be reexamining how we can better take responsibility for our own security online.

### 1Password

Since late 2010, I've been migrating all my online accounts over to using [1Password](https://agilebits.com/onepassword). I can't recommend this application enough. It's really helped me to simplify management of my many online accounts, making it easy to use strong, unique passwords for each without the nightmare of having to remember them all. The way that 1Password can integrate with [Dropbox](https://www.dropbox.com/) makes it easy to keep all my physical devices in sync as far as my online accounts go and the browser extensions available for all the popular browsers are an added convenience making 1Password a 'Why didn't I start using this sooner?' kind of deal for me. Just make sure your master password is damn strong of course, or your account security runs the risk of collapsing like a house of cards.

There was an issue with the 1Password Chrome extension in late 2011, where due to a Chrome update it essentially stopped working for longer than would have been ideal. This was a very annoying problem for me, but the support I received from AgileBits (the company behind 1Password) was very responsive throughout.

The problem with my ISP user account was that it was one of last stragglers that I had yet to get around to switching over to managing with 1Password. This problem has since been rectified.

### Online banking

I deal mainly with two different financial institutions for my banking needs. One of these has quite a developed online banking interface and has had [two-factor authentication](http://en.wikipedia.org/wiki/Two-factor_authentication) for some time. The other is not so developed (to put it politely) and only added the possibility for my account to make use of two-factor authentication this year. Needless to say all my online banking is now protected by two-factor authentication along with strong, unique passwords.

If you're able to register for two-factor authentication with your bank(s) and you haven't taken the time to set it up yet, then I strongly recommend you do take the time, or move to a bank that does provide two-factor authentication for their online banking.

### Google 2-step verification

In 2011, Google introduced their own implementation of two-factor authentication for Google user accounts that they're calling [2-step verification](https://support.google.com/accounts/bin/answer.py?hl=en&answer=180744). I heard about this feature shortly after it was released, thought 'Oh, I should set that up' and promptly never did. Well, now I have and if you own a Google account, you should too.

Google has pretty good documentation on how to set it up, so I'm not going to rehash things here. But some points to highlight:

You can for example, set your main computer to be trusted so that you only have to type the extra verification code once a month on a trusted device.

You need to set up a single use, application specific password for each of the client-side applications that you might use with Google services. Such as if you use a mail client like Thunderbird or Outlook with Gmail, an RSS reading application with Google Reader, the Mail app on an iPhone with Gmail, [Google account sync with Google Chrome](http://support.google.com/chrome/bin/answer.py?hl=en&answer=1181420) and so on. This is a bit of a pain, but you only need to do it once. Plus this also provides extra security in the event that say, your phone is ever stolen, you can revoke the application specific password(s) you used with Google services on your phone so at least the thief can't add insult to injury by potentially taking possession of your Google user account along with your phone.

Speaking of phones, you can also install [Google Authenticator](http://support.google.com/accounts/bin/answer.py?hl=en&answer=1066447) for [iOS](http://en.wikipedia.org/wiki/IOS) and [BlackBerry](http://en.wikipedia.org/wiki/BlackBerry), which ostensibly works similarly to a [SecurID device](http://en.wikipedia.org/wiki/SecurID) as an alternative to getting your verification codes on your phone via [SMS](http://en.wikipedia.org/wiki/SMS). I got a kick out of setting Google Authenticator up on my iPhone. After downloading the app, the setup process is really as simple as holding your phone up to your computer's monitor so that the inbuilt camera can read a [QR code](http://en.wikipedia.org/wiki/QR_code) off the screen. Livin' in the future!

But what happens if you loose your phone? Well you might be pretty screwed, unless you happen to still have access to your Google account on your computer or took note of the set of single-use backup verification codes that you can generate while setting up 2-step verification. I'd recommend putting these codes somewhere *very* safe.

All up, I think it took me about an hour to set up 2-step verification, sorting out all my devices and client-side applications that I use in conjunction with Google services. But I feel the time was well spent[^1].

### TL;DR

If you've been putting off hardening the security of your online accounts, then don't wait for the wakeup call I needed to prompt me to finish the job. Do it soon, or better yet, do it now.

[^1]: The question of whether one should still be using Google services considering the recent changes to their privacy policy is a valid one. But that's a discussion for another day.
