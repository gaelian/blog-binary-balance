---
title: How I've started to contribute to open source
enki_id: 19
tags: [open-source, ruby, rails, development, gems]
---
A couple of weeks ago, I read a post by Brandon Hays called '“Why I still don't contribute to open source”:http://brandonhays.com/blog/2011/05/03/why-i-still-dont-contribute-to-open-source/' wherein Brandon lays out his reasons for not yet having contributed to any [OSS](http://en.wikipedia.org/wiki/Open-source_software) projects. To reiterate Brandon's points:

1. There's no certification, ceremony, or merit badge that says, 'you're ready to contribute to OSS'.
2. It's not obvious where to start.
3. Guidelines often make a maintainer's life easier, and mine harder.
4. Open source is for people who are better at this than me.
5. Trying to contribute and failing makes me feel stupid.
6. There's no time.
7. It's pretty lonely.

I found Brandon's article interesting because I can definitely relate to a lot of what he says.

How do I know I'm ready to hack on an open source project? And if I'm not ready but jump in anyway, won't I potentially just be broadcasting my stupidity across the Interwebs for all to see? How do I know where to start? The code base seems so intimidating and I know nothing about it. Are any hazing rituals involved? Those guys are *so much better* at hacking code than I'd be after ten lifetimes, how the fuck did they get so smart? Or am I just really retarded? I've got my own stuff I want to work on. All these sentiments I've felt at one time or another.

But recently, I've started contributing to open source projects anyway.

The rest of this post is devoted to a couple of small case studies in my open source contributions and the lessons that I have drawn from them, hopefully for the benefit of those of us who may be interested in beginning to contribute to OSS.

### Enki and HTML 5, etc.

A few months ago, I decided to start a blog. Being partial to a bit of Ruby and Rails, I went looking for a solution based on these technologies and finally came to rest on [Enki](http://www.enkiblog.com/) by [Xavier Shay](http://rhnh.net/). Xavier's design philosophy appealed to me, I liked the straight forwardness of Enki. I can also credit my experience with Enki as being probably the largest single factor that got me comfortable with [Git](http://git-scm.com/) and [GitHub](https://github.com/).

As part of sorting out my own blogging needs using Enki as a base, I felt like experimenting with a couple of features:

- An [implementation of the honeypot anti-spam technique](https://github.com/gaelian/enki/tree/honeypot) which is so far working really well for me.
- A [simple way to include a text marker in posts after which the post body is truncated](https://github.com/gaelian/enki/tree/truncate-home-page-posts) for display on the home page (possibly familiar to users of [WordPress](http://wordpress.org/)).

Participating on the Enki mailing list, I noted that Xavier mentioned he was looking for some help to convert Enki to [HTML5](http://en.wikipedia.org/wiki/HTML5). This sounded like something I could tackle, so I put my hand up for it and a little while later had [some changes ready for review](https://github.com/xaviershay/enki/compare/aa3ff...51704). This work wasn't hard, but it was pretty extensive and also required that Xavier update some of the Enki feature/spec files to tidy things up.

A few months later, feeling encouraged by the reception of my previous contributions, I took on an [open issue](https://github.com/xaviershay/enki/issues/7) for Enki and submitted a [patch to close it](https://github.com/xaviershay/enki/compare/75870...e65e1) shortly after.

Contributing to Enki has been enjoyable. I don't think I could have found a better introduction to open source contribution than this small foot print blogging system if I had planned to find an OSS project to contribute to from the beginning. As it was, I really hadn't been aiming to contribute to any OSS project, I was just looking for a blogging platform.

Thanks to Xavier Shay for writing Enki in the way he has and for being receptive and open to contributions from n00bs. His focus on creating a blogging app for developers that relies strongly on version control and hacking code over predefined preference user interfaces and plugin systems is what encouraged me to contribute to this project almost without realising I was doing it at first.

### paypal_adaptive and SSL certificate verification

I'm currently working on a pet project where I'm using the [Adaptive Payments API](https://www.x.com/community/ppx/adaptive_payments) from PayPal. My project is a Rails app so I had a look around to see if anyone has done any [Ruby](http://en.wikipedia.org/wiki/Ruby_%28programming_language%29) based work with Adaptive Payments that I could build on top of. I found a few options, but eventually settled on the [paypal_adaptive gem](https://github.com/tc/paypal_adaptive) by [Tommy Chheng](http://tommy.chheng.com). Tommy describes it as a light wrapper for PayPal's Adaptive Payment API. I chose it because it appeared to be one of the newer Ruby API wrappers for Adaptive Payments so there was a good chance it was still being actively maintained/developed[^1]. And being the light wrapper that it is, it didn't seem too hard to get my head around the code base.

So I installed the gem and went to work. But not too long into integrating paypal_adaptive into my project – while testing my first remote API call to PayPal – I was hit by this error:

```
SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
```

A little googling revealed that this exception is caused by Net::HTTP (the component of the Ruby standard library that paypal_adaptive uses to make remote calls to PayPal) not being able to find a root certificate bundle with which to verify SSL certificates (Net::HTTP can also handle HTTPS calls and that's what paypal_addaptive was trying to do). But I could find no mention of this issue in relation to the use of paypal_adaptive. Why was I hit with this issue and not other users of Tommy's gem, I wondered. I believe the likely answer was because I am using Ruby 1.9.2 and [TLS/SSL](http://en.wikipedia.org/wiki/Transport_Layer_Security) connections done through Net::HTTP in Ruby 1.9.x by default try to verify SSL certificates whereas Ruby 1.8.x by default does not. So anyone using paypal_adaptive with Ruby 1.8.x wouldn't have unknowingly run into this issue.

There's a fair bit of information online about how to get around the 'certificate verify failed' error. Some recommend [just telling Ruby not to verify SSL certificates](http://www.ruby-forum.com/topic/176626#773356) by setting the 'verify_mode' attribute of your Net::HTTP object to 'OpenSSL::SSL::VERIFY_NONE'. But there's good reason why Ruby 1.9.x changed the default behaviour. If you're not verifying the SSL certificate of the party that you're connecting to, then you're only getting part of the benefit of using TLS/SSL. You're still getting the encrypted data transfer but you're not verifying that the the party on the other end of that encrypted data transfer is who you think it is. So for me 'OpenSSL::SSL::VERIFY_NONE' really wasn't an option. A better solution is to let Ruby know where it can find a root certificate bundle. But how best to do this considering that the relevant code is inside the paypal_adaptive gem?

Time for some open source contribution! But before anything else, I wanted to do some recon. I sent Tommy an email discussing this issue, asking what he thought might be the best way to tackle it and whether he'd be interested in a patch from me. After a couple of emails back and forth we'd agreed on a plan of attack and I told Tommy that he could expect a pull request from me on GitHub once I'd got something worth looking at.

I put aside my pet project for a little while and got to work on modifying paypal_adaptive to [include SSL certificate verification](https://github.com/tc/paypal_adaptive/compare/c3b78...12cbb). After my first pull request was accepted, I realised that I had managed to introduce a minor bug in the new SSL certificate verification functionality, and I later submitted [another pull request to fix](https://github.com/tc/paypal_adaptive/compare/1a34c...f06ec) the issue. Broadcasting my stupidity across the Interwebs? Check.

I am now back to my regularly scheduled programming on my own project with the new and improved SSL certificate verification support in paypal_adaptive. Thanks to Tommy Chheng for writing paypal_adaptive and for being a responsive maintainer.

### Lessons learnt

So what can I draw from the above case studies?

#### GitHub rocks!

GitHub works pretty damn well for me. Once I'd gotten a basic familiarity with Git it was only a small step further to get familiar with GitHub. After which, a lot of the uncertainty and potential friction around the practicalities of contributing to any open source project that makes use of Git and GitHub are all neatly removed. Just fork, clone, hack, push, send pull request.

I sometimes get the feeling that some developers wonder what all the fuss is about. After all, people have been hacking code and contributing to open source long before GitHub and its ilk hit the scene. Github is certainly not a necessity, but I can only speak for myself when I say that GitHub has made a real difference in my ability to feel like I'm able to contribute to open source. The guys behind it have done a great job and they deserve all their success[^2].

So the first lesson I might draw would be to find a community enabler like GitHub. If you prefer [Mercurial](http://mercurial.selenic.com/) there's [BitBucket](https://bitbucket.org/). The centralised nature of older version control systems like Subversion and CVS don't seem to lend themselves well to such decentralised development as one sees with Git or Mercurial but I gather there are ways to [bridge older systems](https://github.com/blog/626-announcing-svn-support) at least.

#### You will make mistakes

Many introverted people – counting myself as one – really don't like to look silly, especially not in public. We can be overly self conscious and engaging in activities that contain the promise of some degree of public humiliation does not usually sit well. It's very likely that you will make mistakes from time to time when contributing to open source projects and yes, those mistakes will be publicly visible. But it's pretty damn hard to learn anything without making any mistakes.

I wish there was some secret weapon I could provide here to get deftly past this fear of failure and ridicule. I have none. I can only observe that as I have grown older and more confident within myself, my ability to be comfortable with risking failure in pursuit of my goals seems to have grown, maybe for some people it just takes some time. It certainly helps not to take myself too seriously, a sense of humour about my short comings has helped me when I can summon it. Something else that tends to put things into perspective for me in an inspiring way is an excerpt from one of Theodore Roosevelt's speeches that has come to be known as '“The Man in the Arena”:http://en.wikipedia.org/wiki/Citizenship_in_a_Republic':

> It is not the critic who counts; not the man who points out how the strong man stumbles, or where the doer of deeds could have done them better. The credit belongs to the man who is actually in the arena, whose face is marred by dust and sweat and blood; who strives valiantly; who errs, who comes short again and again, because there is no effort without error and shortcoming; but who does actually strive to do the deeds; who knows great enthusiasms, the great devotions; who spends himself in a worthy cause; who at the best knows in the end the triumph of high achievement, and who at the worst, if he fails, at least fails while daring greatly, so that his place shall never be with those cold and timid souls who neither know victory nor defeat.

#### It's true, scratch your own itch

I realise this isn't a new revelation, but all my open source contributions to date have been of the 'scratch your own itch' type. This really does seem like the most natural way to get into open source contribution. As I found with Enki, doing it this way I almost didn't notice the transition between using the app and contributing to it. If you can find a personal itch to scratch then my experience suggests that this is the easiest way to start.

#### Talk to the maintainer(s) first

I have so far always spoken to the project maintainer before contributing anything to their project. I'm not sure how just shooting pull requests at people out of the blue works, but my guess is it doesn't work as well. The maintainer knows their project best and I've found it's important to clearly understand what their perspective is up front. When contributing to paypal_adaptive, Tommy Chheng actually ended up suggesting what I thought was a much better approach to the SSL certificate verification issue than the one I originally brought to him and from there we built a little on each others ideas to reach the final patch.

In communicating with the maintainer I usually come from a few assumptions:

1. The maintainer is a smart person
2. The maintainer is a busy person
3. The maintainer is a person

Based off these assumptions I then try to keep my messages brief, to the point and respectful.

I sometimes see in myself a reluctance to contact certain developers, particularly those who have some degree of Internet celebrity about them. I assume this reluctance partially stems from the same kind of 'star struck' reaction that might be common in the off-line world when meeting or interacting with someone whose qualities you admire. But beyond this, the thought that these people must get a lot of mail and might just be sick of talking to strangers about their silly little issues can weigh on my mind also. What I have found is that if I just stick to the above assumptions then my interactions (even with celebrity developers, as minor as those interactions may be) tend to go pretty smoothly.

There's another reason why I like to contact maintainers first and it's kind of the same reason why I tend to contact online stores before placing an order with them. I want to scope them out and see how responsive they are and what their style of communication is. In the case of paypal_adaptive, had I never heard back from Tommy Chheng, I then would have known that the better option was probably just to fork his project and continue on my own (see GitHub rocks!).

#### Start small and easy

I've deliberately tried to start off making small and easy contributions. I've also concentrated on contributing to projects that are based on technologies that I already have some familiarity with (Ruby, Rails). This way the organisation of the code base is not entirely foreign to me, I have an existing frame of reference.

I don't have a lot of time and I do have my own stuff that I'm working on, so small and occasional contributions are really all I can commit to for the foreseeable future. I'm not apologising for this and I don't feel that anyone is asking me to.

### Conclusion

I hope this helps someone to feel like they're ready to contribute to open source, even if it's in the small ways that I have now managed. In my niche of the IT industry (i.e. web development) there's so much great open source software out there that we all rely on day in, day out and it feels good to give a little something back.

OSS can look like a shark tank to newbies, but if you take it slow and steady, I think most of the sharks can be avoided.

[^1]: Also, being a Ruby programmer I'm fascinated by anything new and shiny, while simultaneously heedless of any backwards compatibility issues my penchant may engender.

[^2]: I feel some acknowledgement should also go to [Linus Torvalds](http://en.wikipedia.org/wiki/Linus_Torvalds) for developing Git its self. But it's not like any of us will forget that guy's contributions regardless of what happens from here on out.
