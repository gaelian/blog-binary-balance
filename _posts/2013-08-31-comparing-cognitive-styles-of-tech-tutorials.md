---
title: Comparing cognitive styles of tech tutorials
enki_id: 32
tags: [unix, linux, windows, user-interface, development]
---
Historically speaking and development-wise, I've had a lot of experience with technologies that typically rest upon [Unix](http://en.wikipedia.org/wiki/Unix) and [Unix-like](http://en.wikipedia.org/wiki/Unix-like) environments. I primarily used a Windows computer until about 4 years ago (when I primarily switched to a Mac), but even back then I often still felt more comfortable using traditionally Unix based tools for my web development activities on top of Windows when possible[^1]. While I still maintain use of Unix based tools (these days mainly on top of OS X), for the last couple of years I've also dived head first into the end-to-end Microsoft development environment: Windows, Visual Studio, Internet Information Server, SQL Server, Team Foundation Server. I've mostly still stuck to web development, but I've also had the opportunity to do some [VSTO](http://en.wikipedia.org/wiki/Visual_Studio_Tools_for_Office) work and other such things that venture outside of my usual web development bent, while still being within Microsoft's walled development garden.

The purpose of this post is not to compare Unix and Windows [toolchains](http://en.wikipedia.org/wiki/Toolchain), I find the endless *nix vs Windows debate fairly boring and I try to be as practical and nonreligious as possible when it comes to the topic. Rather I will discuss what I think is an interesting difference I've noticed in the way tutorials for these different toolchains are usually delivered.

Some years ago I read a post called '[The Cognitive Style of Unix](http://blog.vivekhaldar.com/post/3339907908/the-cognitive-style-of-unix)' by Vivek Haldar[^2] and I think the topic of my post today has some follow on relevance to this post. The research upon which Vivek bases his post frames the [UI](http://en.wikipedia.org/wiki/User_interface) in terms of internalisation vs externalisation, where an example of a UI that encourages internalisation would be the *nix command line and the applications that are accessible through it, while an example of a UI that encourages externalisation would be say, the Visual Studio [IDE](http://en.wikipedia.org/wiki/Integrated_development_environment).

### Externalised tech tutorials

I've had a fair amount to do with the [Microsoft SharePoint](http://en.wikipedia.org/wiki/Microsoft_SharePoint) platform[^3] over the last few years. One of the first activities for me when working with a new platform/framework/application is to search the Internet for relevant tutorials. Here are a few tutorials that are fairly representative of what one finds when looking for SharePoint knowledge as a developer on the Internet:

- [Walkthrough: Creating an Application Page](http://msdn.microsoft.com/en-us/library/vstudio/ee231557.aspx)
- [Application page in SharePoint 2013 using visual studio 2012](http://www.sharepoint-journey.com/application-page-in-sharepoint-2013.html)
- [Developing Web Parts for Sharepoint 2010](http://www.codeproject.com/Articles/136857/Developing-Web-Parts-for-Sharepoint-2010)

The first tutorial is from Microsoft its self. Because the tools for which the tutorial is written take the externalised approach with their UI, related tutorials must obviously make accommodation. In this example, Microsoft has gone for an explanation consisting of ordered lists of instructions. For example:

1. Start Visual Studio.
2. Open the **New Project** dialog box, expand the **Office/SharePoint** node under the language that you want to use, and then choose the **SharePoint Solutions** node.
3. In the **Visual Studio Installed Templates** pane, choose the **SharePoint 2010 – Empty Project** template. Name the project **MySharePointProject**, and then choose the **OK** button.
4. The **SharePoint Customization Wizard** appears. This wizard enables you to select the site that you will use to debug the project and the trust level of the solution.
5. Choose the **Deploy as a farm solution** option button, and then choose the **Finish** button to accept the default local SharePoint site.

This is to me the least effective way that one could teach an externalised UI. I personally find unnecessary cognitive load resulting when I have to read text instructions then apply them to an externalised UI (or at least an unfamiliar externalised UI, and if I'm familiar with it then I probably have less use for a tutorial). The bolding of things to look out for in the UI doesn't really seem to help me all that much.

The other tutorials I've included are typical examples of those that are found across the SharePoint community. The one thing they have in common is they make liberal use of screen shots to impart knowledge of the externalised UI. A generally better approach than the ordered lists of text, I believe. But as someone who has written the odd tech tutorial, the first thing that sticks out to me is the added effort for tutorial authors, because they not only have to write their tutorial, they have to take screen shots, process them, insert them, give them a value for their alt attribute (which would be appropriate in this case) and upload them. I've also come across situations where the only tutorial I can find is for an older version of the relevant product(s) and the screen shots are just outdated enough to be cryptic.

### Internalised tech tutorials

Here's some internalised UI tutorials:

- [Compiling Ruby 1.9.3 on Debian Squeeze](/2011/10/31/compiling-ruby-1-9-3-on-debian-squeeze) (a perennial visitor's favourite from this very site)
- [How to Use the vi Editor](http://www.washington.edu/computing/unix/vi.html)
- [Getting Started with Rails](http://guides.rubyonrails.org/getting_started.html)

The internalised command line UI engenders a tutorial that provides fairly efficient transfer of knowledge via lots of text. It's easier for the tutorial's author too, I can tell you that much from personal experience. But wait, you say! That third one has screen shots! Yeah, it does. I included that one to illustrate that screen shots are rather more optional than they are required in the case of an internalised UI.

A potential problem with the internalised tech tutorial is the same as it is with the internalised UI: the steeper learning curve for a novice. I would hope that people don't copy/paste and run shell commands off the Internet without understanding what they do, but I guess I can't be sure that this kind of tutorial isn't inadvertently promoting such behaviour[^4]. The internalised tutorial is also subject to becoming stale due to changes made to the tutorial's subject tool but – in my own anecdotal experience at any rate – I've found this to be less of a problem with an internalised tech tutorial than with externalised.

### Unintentional hurdles

So the realisation that interested me enough to write this post relates to the added effort that I see is required to put together a useful externalised tech tutorial over the internalised. It makes me wonder what other unintentional hurdles we may be building for ourselves because of a particular UI paradigm (be it externalised or internalised), not just when directly using our technology but also when engaging in peripheral activities such as transferring knowledge of that technology to those seeking it.

[^1]: Back in those days I tended to use [WAMP](http://en.wikipedia.org/wiki/WAMP) as it seemed to be an option that was a little more friendly on Windows than [Rails](http://en.wikipedia.org/wiki/Ruby_on_Rails) and it's supporting technologies were at the time.

[^2]: Both in the comments of the linked blog post and also over on [Hacker News](https://news.ycombinator.com/item?id=2229833) one can find some dissection of the research that Vivek Haldar cites. As I'm talking today about cognitive styles of a computer UI as it particularly relates to power users and programmers, much of what I read in that discussion is only tangentially applicable to the topic at hand.

[^3]: A platform that provides a level of extensibility rivalled only by its own cruftiness.

[^4]: Looks like I don't have to feel so bad about this possibility anymore: http://explainshell.com/
