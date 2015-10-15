---
title: Compiling Ruby 1.9.2 on Debian Lenny
enki_id: 2
tags: [ruby, debian]
---
I've just recently been setting up my Debian Lenny web server to run Rails apps. So I thought I might document some of what I did for posterity and as a way to give back a little in return for all those blog posts and general instructional texts found on the Internet that have helped me over the years.

[Ruby 1.9.0 is already available in the Debian Lenny repositories](http://packages.debian.org/lenny/ruby1.9), so if you're happy with this version you can of course just install the ruby1.9 package and be done with it. You could also check what backports are available and stuff. But I can't leave well enough alone though so if you're like me, read on.

[Ruby 1.9.2](http://www.ruby-lang.org/en/news/2010/08/18/ruby-1-9.2-released/) has been officially released. Shiny.

Of particular note to me was this entry in the FAQ section:

> The standard library is installed in /usr/local/lib/ruby/1.9.1

Seems a bit weird, but OK. Yay for backwards compatibility I guess?

### Note about RubyGems

RubyGems comes with Ruby 1.9.x so you won't need to install it separately. There is some debate about how RubyGems should work on Debian and the version of RubyGems that is available in the Debian repositories has been customised. I personally prefer just to have the standard RubyGems working in the standard way. So I'm fine with RubyGems coming along with Ruby 1.9.x.

You'll need at least the libmysqlclient15-dev package before installing the mysql gem in order to use MySQL with your Rails apps, but as people don't always use MySQL as their database engine and this post is just about compiling Ruby, I'm not going to get into that stuff here.

### Forward, not backward, upward, not forward and always twirling, twirling, twirling towards freedom

I'm using aptitude for my package management and the root account, so there'll be no 'sudo' command used herein. Adjust your commands accordingly if you don't do things this way.

We will be installing Ruby into the /usr/local directory, change to the /usr/local/src directory:

```bash
# cd /usr/local/src
```

This is where we're going to be doing our work.

Next, we need to ensure that we've got all the required build dependencies for compiling Ruby:

```bash
# aptitude install libc6-dev libssl-dev libreadline5-dev zlib1g-dev
```

The libc6-dev package **should** be already installed on Debian Lenny. But I thought I'd list it anyway.

Download the Ruby 1.9.2 source files:

```bash
# wget ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.2-p0.tar.gz
```

Extract the files from the tar ball:

```bash
# tar xvzf ruby-1.9.2-p0.tar.gz
```

Change into the newly extracted directory:

```bash
# cd ruby-1.9.2-p0
```

Run autoconf:

```bash
# autoconf
```

Run the configure script with the desired options:

```bash
# ./configure —prefix=/usr/local —with-openssl-dir=/usr —with-readline-dir=/usr —with-zlib-dir=/usr
```

Lots of output should fly by as the configure script does its thing.

Now we make:

```bash
# make
```

Even more output should start flying by in your shell terminal.

### Obsessive compulsive software compilation

Usually at this point the next step is to type the 'make install' command, but wait! I must digress for a moment.

The only thing I hate about compiling new software (other than when it doesn't work) is that once I've installed the software into the sub-directories of /usr/local there's often no easy way to uninstall all the files if and when the time comes to do so. Some source includes the ability to run 'make uninstall' from the original directory that you compiled the software in, but in my experience to date, most don't. I could install everything into a single sub-directory (such as /usr/local/ruby) but this can cause issues later on when other software might be expecting compiled binaries and other files to be in certain locations when they're not. There are mitigations for this issue, but I think I know of a better solution.

Debian package management is awesome. It allows you to easily add and remove packages at will with succinct commands, while dependencies are largely taken care of transparently. But what if like me, you're a bit obsessive compulsive about keeping your file system neat and you'd like to have a similar capability even when compiling your own stuff from source? Debian to the rescue! If you don't already have it, grab the checkinstall package:

```bash
# aptitude install checkinstall
```

The checkinstall package will allow you to install your newly compiled software as a package that can then be managed (and most importantly to me, removed) like any other Debian package.

Now we're going to install Ruby using checkinstall:

```bash
# checkinstall -D make install
```

The '-D' flag means that we're going to build a Debian package. Now checkinstall will ask you if you want to create some documentation for your new Ruby package:

```bash
The package documentation directory ./doc-pak does not exist.
Should I create a default set of package docs? [y]:
```

You don't have to do this, but it might be kind of fun. So let's answer 'y'.

Now you'll get:

```bash
Please write a description for the package.
End your description with an empty line or EOF.
>>
```

So we can type in a description, maybe something like:

```bash
>> Ruby Programming Language (MRI 1.9.2)
```

Then hit Enter and then Enter again as the description ends when you enter an empty line.

Next up you should get something that looks like this:

```bash
*****************************************
**** Debian package creation selected ***
*****************************************

This package will be built according to these values:

0 -  Maintainer: [ root@server.domain.com ]
1 -  Summary: [ Ruby Programming Language (MRI 1.9.2) ]
2 -  Name:    [ ruby-1.9.2 ]
3 -  Version: [ p0 ]
4 -  Release: [ 1 ]
5 -  License: [ GPL ]
6 -  Group:   [ checkinstall ]
7 -  Architecture: [ amd64 ]
8 -  Source location: [ ruby-1.9.2-p0 ]
9 -  Alternate source location: [  ]
10 - Requires: [  ]
11 - Provides: [ ruby-1.9.2 ]

Enter a number to change any of them or press ENTER to continue:
```

If you're happy with all that, then just hit Enter again. If you want to change anything, type the number of the line you want to change and hit Enter. Let's change the email address of the maintainer, type '0', then Enter. You should get:

```bash
# Enter the maintainer's name and e-mail address:
>>
```

Let's just type in an email address and hit Enter:

```bash
# Enter the maintainer's name and e-mail address:
>> not@any.com
```

We should get back to:

```bash
This package will be built according to these values:

0 -  Maintainer: [ not@any.com ]
1 -  Summary: [ Ruby Programming Language (MRI 1.9.2) ]
2 -  Name:    [ ruby-1.9.2 ]
3 -  Version: [ p0 ]
4 -  Release: [ 1 ]
5 -  License: [ GPL ]
6 -  Group:   [ checkinstall ]
7 -  Architecture: [ amd64 ]
8 -  Source location: [ ruby-1.9.2-p0 ]
9 -  Alternate source location: [  ]
10 - Requires: [  ]
11 - Provides: [ ruby-1.9.2 ]

Enter a number to change any of them or press ENTER to continue:
```

Assuming we're now happy, we can just hit Enter.

Another bunch of output should fly by in your terminal until it stops at:

```bash
Some of the files created by the installation are inside the build
directory: /usr/local/src/ruby-1.9.2-p0

You probably don't want them to be included in the package,
especially if they are inside your home directory.
Do you want me to list them?  [n]:
```

I'm not really interested in a list, so I type 'n' and checkinstall comes back with:

```bash
Should I exclude them from the package? (Saying yes is a good idea) [y]:
```

I'll take checkinstall's word for it and type 'y'. A bit more output is generated and then:

```bash
**********************************************************************

 Done. The new package has been installed and saved to

 /usr/local/src/ruby-1.9.2-p0/ruby-1.9.2_p0-1_amd64.deb

 You can remove it from your system anytime using:

      dpkg -r ruby-1.9.2

**********************************************************************
```

So as you can see, a new .deb package has been created in the Ruby source directory that you can – in the future and should you wish – install in a similar way to any local .deb package. The only caveat is that I gather any dependencies of a package created in this way are not taken into account when it is installed.

But the important thing from my point of view is that Ruby 1.9.2 is now installed on my server and I can remove it quite easily by typing:

```bash
# dpkg -r ruby-1.9.2
```

Let's just verify that Ruby is all happy and working:

```bash
# ruby -v
```

This should produce something similar to:

```bash
# ruby 1.9.2p0 (2010-08-18 revision 29036) [x86_64-linux]
```

OK, and typing:

```bash
ruby -ropenssl -rzlib -rreadline -e "puts 'Ruby appears to be working OK with openssl, zlib and readline support.'"
```

You should get the expected output of:

```bash
# Ruby appears to be working OK with openssl, zlib and readline support.
```

Lastly, let's check out RubyGems:

```bash
# gem env
```

This should produce something like:

```bash
RubyGems Environment:
  - RUBYGEMS VERSION: 1.3.7
  - RUBY VERSION: 1.9.2 (2010-08-18 patchlevel 0) [x86_64-linux]
  - INSTALLATION DIRECTORY: /usr/local/lib/ruby/gems/1.9.1
  - RUBY EXECUTABLE: /usr/local/bin/ruby
  - EXECUTABLE DIRECTORY: /usr/local/bin
  - RUBYGEMS PLATFORMS:
    - ruby
    - x86_64-linux
  - GEM PATHS:
     - /usr/local/lib/ruby/gems/1.9.1
     - /root/.gem/ruby/1.9.1
  - GEM CONFIGURATION:
     - :update_sources => true
     - :verbose => true
     - :benchmark => false
     - :backtrace => false
     - :bulk_threshold => 1000
  - REMOTE SOURCES:
     - http://rubygems.org/
```

There you have it. Ruby 1.9.2 and RubyGems 1.3.7 installed on Debian Lenny, using checkinstall for easy removal.
