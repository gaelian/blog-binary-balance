---
title: Compiling Nginx with Phusion Passenger on Debian Lenny
enki_id: 3
tags: [debian, rails, ruby, phusion-passenger]
---
After [compiling Ruby 1.9.2 on my Debian Lenny web server]({% post_url 2010-08-28-compiling-ruby-1-9-2-on-debian-lenny %}), the next thing I wanted to do was get an actual web server going for serving up Rails apps. I've been an Apache user for a long time but I'd been hearing about [Nginx](http://nginx.org/) and how it's kicking ass and taking names, so I thought I might give it a go. I also needed some way to hook Rails up with Nginx.

Back in the day, it could be a real pain in the ass to deploy Rails apps. There was [mod_ruby](http://www.modruby.net/en/) for Apache which tried to be for Ruby what mod_php is for PHP. But it had issues when trying to run multiple applications and the project seems pretty dead.

For a time Zed Shaw's original Mongrel web server was the popular choice. One could set up a number of Mongrel instances working as application servers and have another web server, such as Apache sitting in front and proxying requests back to the Mongrels. But times have changed and Zed has moved on to new things (one of which is his [Mongrel2 web server](http://sheddingbikes.com/posts/1283412933.html) be interesting to see how that project matures).

But as I wanted to try out Nginx, I settled on [Phusion Passenger](http://www.modrails.com/) (AKA mod_rails or mod_rack). It's a module that can be used with either Apache or Nginx and will take care of the action between a Rails app (or other Ruby based web app that follows the [Rack](http://en.wikipedia.org/wiki/Rack_%28web_server_interface%29) interface) and the web server.

Phusion Passenger can make deploying a Rails app about as straight forward as using PHP and MySQL/Postgres with Apache.

### Enough with the history lesson, get on with it god damnit!

OK, so Nginx is available in the Debian repositories. Phusion Passenger can also be more automagically installed using [other methods](http://www.modrails.com/install.html). But I wanted to get a better understanding of what's happening a little deeper under the hood, so I went for the more manual procedure.

I'm going to assume you've got Ruby installed and working. I'm also using aptitude for my package management and the root account, so there'll be no 'sudo' command used herein. Adjust your commands accordingly if you don't do things this way.

### If you wanna be a passenger

Grab the Phusion Passenger gem:

```bash
# gem install passenger
```

That's all we have to do with Passenger for now.

### Drivers, start your Nginx

Change to the /usr/local/src directory:

```bash
# cd /usr/local/src
```

This is where we're going to be doing our work.

Download the Nginx source:

```bash
# wget http://nginx.org/download/nginx-0.7.67.tar.gz
```

At the time of this writing, 0.7.67 was the latest stable version of Nginx.

Extract the files:

```bash
# tar xvzf nginx-0.7.67.tar.gz
```

Change into the newly extracted directory:

```bash
# cd nginx-0.7.67
```

Take care of some dependencies:

```bash
# aptitude install libpcre3 libpcre3-dev libpcrecpp0 libssl-dev zlib1g-dev
```

Run the configure script:

```bash
# ./configure —sbin-path=/usr/local/sbin —with-http_ssl_module —add-module=`passenger-config —root`/ext/nginx
```

Note that the 'passenger-config —root' command you can see nested in there will be substituted with the path to the root of the Phusion Passenger gem that we installed earlier, it's just a shortcut so that you don't have to type in the full path. On my system, 'passenger-config —root' would output '/usr/local/lib/ruby/gems/1.9.1/gems/passenger-2.2.15' but this may differ for you if you have Ruby installed somewhere else, you use a different version of Ruby or you install your gems somewhere else.

A whole bunch of output should fly by in the terminal and you will eventually be presented with a summary of the Nginx configuration that looks something like this:

```bash
Configuration summary
  + using system PCRE library
  + using system OpenSSL library
  + md5: using OpenSSL library
  + sha1 library is not used
  + using system zlib library

  nginx path prefix: "/usr/local/nginx"
  nginx binary file: "/usr/local/sbin"
  nginx configuration prefix: "/usr/local/nginx/conf"
  nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
  nginx pid file: "/usr/local/nginx/logs/nginx.pid"
  nginx error log file: "/usr/local/nginx/logs/error.log"
  nginx http access log file: "/usr/local/nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
```

You might want to take note of this summary, as it can come in handy for knowing where config files are located etc.

Now we make:

```bash
# make
```

More output should fly by.

Usually the next step is to type “make install” but as I've mentioned elsewhere on this blog, I'm a bit obsessive about keeping my file system clean and this is Debian, so I like to use checkinstall to wrap my compiled software up into a neat Debian package that can later be more easily removed.

If you don't have checkinstall, grab it:

```bash
# aptitude install checkinstall
```

And we can press on:

```bash
# checkinstall -D make install
```

The '-D' flag means that we're going to build a Debian package. Now checkinstall will ask you if you want to create some documentation for your new Nginx package:

```bash
The package documentation directory ./doc-pak does not exist.
Should I create a default set of package docs? [y]:
```

You don't have to do this but I'm going to, just for the hell of it. So let's answer 'y'.

Next you'll get:

```bash
Please write a description for the package.
End your description with an empty line or EOF.
>>
```

Type a description, something like:

```bash
>> Nginx HTTP and reverse proxy server (0.7.67)
```

Then hit Enter and then Enter again as the description ends when you enter an empty line.

You should get something that looks like this:

```bash
*****************************************
**** Debian package creation selected ***
*****************************************

This package will be built according to these values:

0 -  Maintainer: [ root@server.domain.com ]
1 -  Summary: [ Nginx HTTP and reverse proxy server (0.7.67) ]
2 -  Name:    [ nginx ]
3 -  Version: [ 0.7.67 ]
4 -  Release: [ 1 ]
5 -  License: [ GPL ]
6 -  Group:   [ checkinstall ]
7 -  Architecture: [ amd64 ]
8 -  Source location: [ nginx-0.7.67 ]
9 -  Alternate source location: [  ]
10 - Requires: [  ]
11 - Provides: [ nginx ]

Enter a number to change any of them or press ENTER to continue:
```

If you're happy with all that, then just hit Enter again. If you want to change anything, type the number of the line you want to change and hit Enter. Let's change the email address of the maintainer, type '0', then Enter. You should get:

```bash
Enter the maintainer's name and e-mail address:
>>
```

Type in an email address and hit Enter:

```bash
Enter the maintainer's name and e-mail address:
>> not@any.com
```

We should end up back at:

```bash
This package will be built according to these values:

0 -  Maintainer: [ not@any.com ]
1 -  Summary: [ Nginx HTTP and reverse proxy server (0.7.67) ]
2 -  Name:    [ nginx ]
3 -  Version: [ 0.7.67 ]
4 -  Release: [ 1 ]
5 -  License: [ GPL ]
6 -  Group:   [ checkinstall ]
7 -  Architecture: [ amd64 ]
8 -  Source location: [ nginx-0.7.67 ]
9 -  Alternate source location: [  ]
10 - Requires: [  ]
11 - Provides: [ nginx ]

Enter a number to change any of them or press ENTER to continue:
```

Assuming we're now happy, we can just hit Enter.

Another bunch of output should fly by in your terminal until you get something that looks a bit like this:

```bash
**********************************************************************

 Done. The new package has been installed and saved to

 /usr/local/src/nginx-0.7.67/nginx_0.7.67-1_amd64.deb

 You can remove it from your system anytime using:

      dpkg -r nginx

**********************************************************************
```

So as you can see, a new .deb package has been created in the Nginx source directory that you can – in the future and should you wish – install in a similar way to any local .deb package. The only caveat is that I gather any dependencies of a package created in this way are not taken into account when it is installed.

Let's see if Nginx is present:

```bash
# nginx -v
```

Should output:

```bash
nginx version: nginx/0.7.67
```

In the words of Mr Burns: “excellent”.

Next time I will cover some post installation configuration for Nginx – namely creating an init script, setting up virtual hosts, enabling the Phusion Passenger module and actually running a Rails app.
