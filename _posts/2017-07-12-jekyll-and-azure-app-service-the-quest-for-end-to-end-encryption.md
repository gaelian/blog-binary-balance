---
title: 'Jekyll and Azure App Service: the quest for end-to-end encryption'
tags: [security, cdn, tls, privacy, github, jekyll, ruby, azure]
---
It's sad but true that my last blog post here was more than a year ago, how time flies. But, I'm not dead ([as Pink once said](https://en.wikipedia.org/wiki/I%27m_Not_Dead)) and I finally have some time to write an update to my [previous post](/2016-03-27-jekylls-and-cdns-and-github-pages-oh-my) - or another instalment of a perennial favourite post topic of mine: where and how is my blog hosted now?!

In my last post I left things with my blog running on [GitHub Pages](https://pages.github.com/) for hosting and [Jekyll](https://jekyllrb.com/) for content management. Well, I'm still using Jekyll but I have now moved to [Azure App Service](https://azure.microsoft.com/en-au/services/app-service/) for hosting. This isn't a free option but I happen to have some Azure credits hanging around for various reasons and I thought I may as well make use of them to see what I could sort out for my blog on the Azure platform and finally get that proper end-to-end [SSL/TLS encryption](https://en.wikipedia.org/wiki/Public_key_certificate) I've been wanting for some time now.<!--more-->

It's worth noting that for what I needed (custom domain, SSL/TLS) the smallest/cheapest App Service plan I could get away with is what's known as [the 'Basic' (B1) App Service plan](https://azure.microsoft.com/en-au/pricing/details/app-service/plans/).

I wanted to use Azure App Service rather than a full-blown virtual server because it's cheaper (to help make the most of my Azure credits) but also because I want to minimise server admin/maintenance work, I never really want a full server I need to manage if I can get away with something else. On the face of it, this approach seems non-viable because Jekyll relies on [Ruby](https://www.ruby-lang.org/en/downloads/) and Ruby isn't supported [OOTB](https://en.wikipedia.org/wiki/Out_of_the_box_(feature)) on App Service instances. However, it turns out that standard App Service instances - as opposed to the Linux-based ones - are still running a (cut down) Windows underneath, with a mostly functional command line and thanks to a few helpful blog posts ([the most up to date one I found being from Khalid Abuhakmeh](https://rimdev.io/deploying-jekyll-to-windows-azure-app-services/)), I was able to get Ruby and the various other Jekyll dependencies installed on a standard App Service instance[^1]. You might think that using one of the Linux-based instances would have been easier if I needed Ruby... I tried but at the moment at least, I did not find it easier. I had some issues with the `path` not working as I would have expected (once I actually figured out how to get to a console session for the Linux instance, that is) and didn't feel like putting the time in to try and work out what was happening. Plus the Linux App Service instances are still in public preview and not entirely stable yet.

Running the scripts from Khalid's previously linked post worked fine other than one issue I ran into: I had updated the script to install Ruby version 2.3.3 on my App Service instance. This was a fairly recent version, the script was already looking [on bintray where 2.3.3 is the latest version available at the time of this writing](https://bintray.com/oneclick/rubyinstaller/rubyinstaller) and while I was getting things working I wanted minimal change to the script. I ended up with this:

```shell_session
@if "%SCM_TRACE_LEVEL%" NEQ "4" @echo off

REM Put Ruby in Path
REM You can also use %TEMP% but it is cleared on site restart. Tools is persistent.
SET PATH=%PATH%;D:\home\site\deployments\tools\r\ruby-2.3.3-x64-mingw32\bin

REM I am in the repository folder
pushd D:\home\site\deployments
if not exist tools md tools
cd tools
if not exist r md r
cd r
if exist ruby-2.3.3-x64-mingw32 goto end

echo No Ruby, need to get it!

REM Get Ruby
REM 64bit
curl -o ruby233.zip -L https://bintray.com/artifact/download/oneclick/rubyinstaller/ruby-2.3.3-x64-mingw32.7z?direct
REM Azure puts 7zip here!
echo START Unzipping Ruby
SetLocal DisableDelayedExpansion & d:\7zip\7za x -xr!*.ri -y ruby233.zip > rubyout
echo DONE Unzipping Ruby

REM Get DevKit to build Ruby native gems
REM If you don't need DevKit, rem this out.
curl -o DevKit.zip http://cdn.rubyinstaller.org/archives/devkits/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe
echo START Unzipping DevKit
d:\7zip\7za x -y -oDevKit DevKit.zip > devkitout
echo DONE Unzipping DevKit

REM Init DevKit
ruby DevKit\dk.rb init

REM Tell DevKit where Ruby is
echo --- > config.yml
echo - D:/home/site/deployments/tools/r/ruby-2.3.3-x64-mingw32 >> config.yml

REM Setup DevKit
ruby DevKit\dk.rb install

popd

:end

REM Need to be in Reposistory
cd %DEPLOYMENT_SOURCE%
cd

call gem install bundler

ECHO Bundler install (not update!)
call bundle install

cd %DEPLOYMENT_SOURCE%
cd

ECHO Running Jekyll
call bundle exec jekyll build

REM KuduSync is after this!
```

However, locally I was using Ruby 2.4.0 and this was the version originally specified in my site's `Gemfile`:

```ruby
source 'https://rubygems.org'
ruby '2.4.0'

gem 'github-pages'
```

This stopped the call to `bundle install` in `getruby.cmd` from successfully completing on my App Service instance until I changed the Ruby version specified in my `Gemfile` to match the version of Ruby `getruby.cmd` had installed (i.e. 2.3.3) and deployed that change to Azure. At some point I will no doubt update to a newer version of Ruby on my App Service instance but for now, 2.3.3 is fine.

One more little gotcha that I'm glad I noticed is that because my blog was now being served up by [IIS](https://en.wikipedia.org/wiki/Internet_Information_Services), the fact that I had set my Jekyll [permalinks](https://jekyllrb.com/docs/permalinks/) to have no file extension meant that all my blog posts were returning a 404 error. [Scott Hanselman](https://twitter.com/shanselman) to the rescue with [an old blog post about how to use the IIS Url Rewrite Module](https://www.hanselman.com/blog/RedirectingASPNETLegacyURLsToExtensionlessWithTheIISRewriteModule.aspx) to do exactly what I was wanting to do: rewrite a request for `/foo` to the underlying `foo.html`. Adding a `web.config` file containing the appropriate rewrite rules to the root of my app sorted this issue.

So I had Jekyll running successfully on Azure App Service and at this stage I was [deploying to App Service from a local Git repository](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-deploy-local-git). Which was OK for testing, but this deployment method doesn't support [SSH](https://help.github.com/articles/connecting-to-github-with-ssh/), which just will not do, right!? I'll come back to this point later.

## Let's Encrypt, custom domain and end-to-end SSL/TLS encryption

Onto sorting out my custom domain and getting proper SSL/TLS encryption!

I wanted to sort out one of those cool, automated and free [Let's Encrypt](https://letsencrypt.org/) certificates for use with my domain, [instructions for adding a custom domain to an App Service instance are over in the Azure documentation](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-tutorial-custom-domain). I went with a CNAME record as I currently also use a `blog` sub-domain for this site). As my domain was already in use I needed to be pretty careful when making DNS changes as such changes could easily cause unexpected downtime[^2]. I also wanted to consolidate my DNS configuration back on one provider ([DNSimple](https://dnsimple.com/)), as my DNS settings (for this and other sites I manage) were split across DNSimple and [CloudFlare](https://www.cloudflare.com/), from the last time I did some work on my hosting arrangements. Suffice it to say that I got my custom domain successfully added to my App Service instance after some DNS record wrangling. Curiously, the Azure Portal did not immediately list the custom domain as being added to my App Service instance. It told me via a pop-up message that the custom domain was added successfully and it none the less effectively did seem to have been be added immediately after I completed the procedure. But for me it just took a while for it to show up under 'Custom Domains' within the App Service instance in the Azure Portal [UI](https://en.wikipedia.org/wiki/User_interface), which I found a little confusing.

Actually getting a Let's Encrypt certificate sorted out for an App Service instance is not 'one-click' easy, but it's made significantly easier by an Azure site extension called [Azure Let's Encrypt](https://www.siteextensions.net/packages/letsencrypt) by [Simon JK Pedersen](https://github.com/sjkp). Instructions on how to install and configure this extension can be found from [Nick Molnar](https://gooroo.io/GoorooTHINK/Article/16420/Lets-Encrypt-Azure-Web-Apps-the-Free-and-Easy-Way/21872). The only problem I had following Nick's instructions was when trying to connect to my Azure tenant and create a new AD application using PowerShell ISE, I was getting the error `Run Login-AzureRmAccount to login.` even though I had already logged in successfully. The fix for me was to run the `Update-Module` PowerShell command to update all modules and then restart my local machine [as described by Darren Robinson](https://blog.kloud.com.au/2016/07/12/powershell-error-run-login-azurermaccount-to-login-in-azurerm-when-already-logged-in/) which I gather sorts out some issue related to Azure PowerShell module install paths being wrong in some cases or something.

So now I had my custom domain along with a cool free Let's Encrypt SSL/TLS certificate all set up. Noice.

As mentioned in Nick Molnar's previously linked post, Let's Encrypt certificates expire after three months and the expectation is that site admins will automate the process of renewing their certificate(s), so it's lucky that the Azure Let's Encrypt site extension helps with this by creating a WebJob that renews your certificate for you 22 days before expiration ([according to the README on the project repository](https://github.com/sjkp/letsencrypt-siteextension)), as Let's Encrypt sends reminder emails 20 days before. The only thing I'd add is that for this WebJob of type 'Continuous' to keep running and actually renew the certificate when the time comes, the 'Always On' setting in the App Service instance's 'Application Settings' section in the Azure Portal needs to be set to 'On'.

And the final touch, making sure that any non-secure (HTTP) requests to my site were redirected to the secure equivalent (HTTPS). Turns out the easiest way to do this for Azure App Service is the [Redirect HTTP to HTTPS](https://www.siteextensions.net/packages/RedirectHttpToHttps/) Azure site extension by [Greg Hogan](https://github.com/gregjhogan/). This little extension worked great once I'd added a binding to my Let's Encrypt certificate under 'SSL Certificates' for my App Service instance. To be clear, the Let's Encrypt certificate was working before I added this binding but not the redirect from HTTP requests to HTTPS. So it would appear that adding this certificate binding is not strictly required when using the Azure Let's Encrypt site extension. But to get the Redirect HTTP to HTTPS site extension working, it seems necessary[^3]. This does raise the question though of whether me having manually added this certificate binding will cause any issue when the time comes for my certificate to be automatically renewed by the WebJob. I guess I'll find out in a couple of months time or so.

## Deployment

I didn't like the idea of having to use a username and password to deploy to Azure App Service from my local Git repository. Another option is to deploy to App Service via GitHub. The process works more or less [as described on the GitHub blog](https://github.com/blog/2056-automating-code-deployment-with-github-and-azure). The UI on the Azure side of things has changed slightly, it's 'Deployment Options' not 'Continuous Deployment' within the App Service instance now. But once set up, you can push changes to your repo on GitHub and those changes auto-magically get deployed to the connected Azure App Service instance, which is pretty neat. I still keep a repo for this site over on GitHub even though I'm not using GitHub Pages any more, so this option is ideal for me.

## A word on local blog tooling

On my local machine I have my local Git repository for my blog along with [Visual Studio Code](https://code.visualstudio.com/)[^4], which is free, works great on MacOS and comes with a nice [Markdown preview feature](https://code.visualstudio.com/docs/languages/markdown). There's also a [Code SpellChecker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) extension available for VS Code, which I have installed.

## Perfection?

For how I want to write and publish these days, this set up is as close to perfect as I've yet found. Now I just need to find more time to write. :P

[^1]: I would also note that there is [an Azure site extension called Jekyll](https://www.siteextensions.net/packages/JekyllExtension/) from [Cory Fowler](https://twitter.com/cfowlermsft) that automates the process of installing Jekyll and the required dependencies for Azure App Service. But as of the time of this writing, [the extension needs an update before it is usable](https://twitter.com/cfowlerMSFT/status/883242486893690881) and I wanted to get things sorted now, so I went with the more manual install process.

[^2]: Not that downtime really matters that much for this little blog site, but old habits die hard I guess.

[^3]: Before I manually added this certificate binding, installing Redirect HTTP to HTTPS site extension and restarting my App Service instance caused a 502 server error. Adding the binding then re-installing the Redirect HTTP to HTTPS site extension worked.

[^4]: Visual Studio Code is a surprisingly awesome text editor, it's almost like Microsoft is listening to what developers really want these days.
