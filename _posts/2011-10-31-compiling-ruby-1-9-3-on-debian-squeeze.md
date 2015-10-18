---
title: Compiling Ruby 1.9.3 on Debian Squeeze
enki_id: 23
tags: [ruby, debian, rvm]
---
There's not a huge amount of Debian specific Ruby/Rails tutorials on the web. I've written a few in the past and with the release of Debian Squeeze and the recent [official release of Ruby 1.9.3](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/40527) I thought it might be time to do another one.<!--more-->

[Ruby 1.9.2 is already available in the Debian Squeeze repositories](http://packages.debian.org/squeeze/ruby1.9.1), so if you're happy with just using what's already easily available you can of course just install the ruby1.9.1 (or [ruby1.9.1-full](http://packages.debian.org/squeeze/ruby1.9.1-full)) package and be done with it. Somewhat confusingly these packages are named with the version number 1.9.1, but this is apparently to indicate compatibility. Installing either package will give you Ruby 1.9.2.

### Ninja compilation

However, I think there's a better way to run Ruby and manage gems than just installing one of the above mentioned packages. What I'm going to detail in this post will not be an entirely traditional way of compiling Ruby. I'm going to be outlining the way of setting up Ruby that I now prefer and that I think is generally considered a better practice option in the Ruby community. Today I'll be installing and setting up [RVM](http://beginrescueend.com/). From the previous link:

> RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems.

RVM allows you to run an arbitrary number of Ruby interpreters, be they different versions of MRI ([Matz's Ruby Interpreter](http://en.wikipedia.org/wiki/Ruby_MRI)), or entirely different Ruby implementations, such as [REE](http://www.rubyenterpriseedition.com/) or [JRuby](http://jruby.org/) for example, each able to have an arbitrary number of 'gem sets' (which are exactly what they sound like) and you can easily switch between them all. It brings a whole new level of flexibility to using Ruby. [Nice](http://www.youtube.com/watch?v=kIfOjkB17BA).

RVM does take a little getting used to, but for me the slightly extended learning curve has been well worth it.

### Assumptions

I will be starting with a stock Debian Squeeze install, I downloaded the [debian-6.0.3-amd64-DVD-1.iso](http://cdimage.debian.org/debian-cd/6.0.3/amd64/bt-dvd/) image and installed from this image.

I'll be using aptitude for package management and 'sudo' with a non-root user account. Adjust your commands accordingly if you don't do things this way.

I'm going to list terminal commands, but I'm not going to list irrelevant output, I'm assuming a basic working knowledge of Debian or at least a Debian-derived Linux distro.

#### Note on RubyGems

I won't be using the RubyGems Debian package. I personally prefer just to have RubyGems working in the more typical way. So I'm fine with RubyGems coming along with Ruby 1.9.x.

### Installing the system Ruby

So let's pop open a terminal and get down to it.

The first thing to do before we get into any real RVM business is install what in RVM terminology is known as the 'system Ruby'. It does't really matter what version of Ruby is installed as the system Ruby, so I'm going to go with what's available from Squeeze's 'ruby' dependency package, which is Ruby 1.8.7 p302.

Install Ruby 1.8.7:

```bash
$ sudo aptitude install ruby
```

Easy. I love the Debian package management system, although it doesn't always love me back.

### Installing RVM

There's two ways of installing RVM:

- Single-User installation
- Multi-User installation

Single-User installation is good for a development environment, RVM will be installed into your $HOME.

Multi-User installation will install RVM in a system-wide capacity, which will (not surprisingly, considering the name) be better for a multi-user environment or perhaps a remote staging/production server set-up. The Multi-User install used to be quite a pain in the ass to get working, but nowadays it seems that much of the pain in the Multi-User install has been alleviated.

I'm going to go with a Single-User install for this post, but the procedure for a Multi-User install is not that different. If you're wanting Multi-User, then you're probably going to find the [RVM install guide](http://beginrescueend.com/rvm/install/) useful.

#### Prerequisites

There's a few prerequisites that RVM requires, so we'll now install any that Squeeze doesn't already have by default[^1]:

```bash
$ sudo aptitude install curl git subversion
```

#### Install RVM using Git

[Git](http://git-scm.com/) is my version control system of choice and I'm reasonably familiar with it. However, whole books have been written on the massive beast that is Git and a full explanation is well outside the scope of this post. If you aren't a Git user then fear not, there's really not a whole lot one needs to do with Git in relation to using RVM.

Install [RVM from GitHub](https://github.com/wayneeseguin/rvm):

```bash
$ bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
```

Note that I didn't use 'sudo' with the above command. What just happened was that RVM was installed into your $HOME, in a hidden directory called '.rvm'. But you don't have to take my word for it. List the .rvm directory:

```bash
$ ls -la ~/ | grep .rvm
drwxr-xr-x 24 gaelian gaelian 4096 Oct 31 18:51 .rvm
```

#### Loading RVM into your terminal session as a function

Previously, you'd have to add a line to your ~/.profile or your ~/.bash_profile file (whichever you're using) like so:

```bash
$ echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.profile
```

But RVM now adds the required line to your ~/.bashrc file automagically. So there's no need for the above command anymore. If you want to see the line that's been added to ~/.bashrc for you:

```bash
$ cat ~/.bashrc | grep rvm
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
```

In any case, you should now close your current terminal session and open a new one so that ~/.bashrc gets reloaded, which should activate RVM.

#### Test it out

Check if RVM has been loaded correctly:

```bash
$ type rvm | head -1
```

If all is well, this should output:

```bash
rvm is a function
```

You can now also run:

```bash
$ rvm requirements
```

Which will show you a helpful dialog that lists the requirements that you may need, specific to Linux and relative to what Ruby implementation you're interested in. I'd recommend having a quick read.

If you did read the output from the 'rvm requirements' command, you'll know that for MRI (i.e. Ruby 1.9.3), [Rubinius](http://rubini.us/) and REE, we'll also need to install a few more packages that don't come standard with Squeeze:

```bash
$ sudo aptitude install build-essential libreadline6-dev git-core zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf automake libtool bison
```

And that's it! You're now running RVM!

### Um... neat, but I wanted Ruby 1.9.3!

And I am a man of my word!

Output a list of all Rubies known to RVM:

```bash
$ rvm list known
```

This should output something like:

```bash
# MRI Rubies
[ruby-]1.8.6[-p420]
[ruby-]1.8.6-head
[ruby-]1.8.7[-p352]
[ruby-]1.8.7-head
[ruby-]1.9.1-p378
[ruby-]1.9.1[-p431]
[ruby-]1.9.1-head
[ruby-]1.9.2-p180
[ruby-]1.9.2[-p290]
[ruby-]1.9.2-head
[ruby-]1.9.3-preview1
[ruby-]1.9.3-rc1
[ruby-]1.9.3[-p0]
[ruby-]1.9.3-head
ruby-head

# GoRuby
goruby

# JRuby
jruby-1.2.0
jruby-1.3.1
jruby-1.4.0
jruby-1.6.1
jruby-1.6.2
jruby-1.6.3
jruby-1.6.4
jruby[-1.6.5]
jruby-head

# Rubinius
rbx-1.0.1
rbx-1.1.1
rbx-1.2.3
rbx-1.2.4
rbx[-head]
rbx-2.0.0pre

# Ruby Enterprise Edition
ree-1.8.6
ree[-1.8.7][-2011.03]
ree-1.8.6-head
ree-1.8.7-head

# Kiji
kiji

# MagLev
maglev[-26852]
maglev-head

# Mac OS X Snow Leopard Only
macruby[-0.10]
macruby-nightly
macruby-head

# IronRuby -- Not implemented yet.
ironruby-0.9.3
ironruby-1.0-rc2
ironruby-head
```

All these rubies can be yours! Let's install MRI 1.9.3:

```bash
$ rvm install 1.9.3
```

Once you're back at the command prompt, switch to using Ruby 1.9.3:

```bash
$ rvm use 1.9.3
Using /home/gaelian/.rvm/gems/ruby-1.9.3-p0
```

Now verify that we're using 1.9.3:

```bash
$ ruby -v
ruby 1.9.3p0 (2011-10-30 revision 33570) [x86_64-linux]
```

So what just happened? Well, when you told RVM to use Ruby 1.9.3, RVM did some magic behind the scenes and switched you over to using the Ruby 1.9.3 interpreter installed within the ~/.rvm directory. Let's look:

```bash
$ which ruby
/home/gaelian/.rvm/rubies/ruby-1.9.3-p0/bin/ruby
```

So Ruby 1.9.3 has been installed to '/home/gaelian/.rvm/rubies/ruby-1.9.3-p0/'. Astute readers may have also noticed that the directory where gem sets can be found is '/home/gaelian/.rvm/gems/ruby-1.9.3-p0'. But more on gem sets later.

If you want to switch back to the system Ruby (remember the system Ruby was MRI 1.8.7 p302):

```bash
$ rvm use system
Now using system ruby.
```

And...

```bash
$ ruby -v
ruby 1.8.7 (2010-08-16 patchlevel 302) [x86_64-linux]
```

Now let's install Rubinius 1.2.4:

```bash
$ rvm install rbx-1.2.4
```

When you're again back at the command prompt, we can switch to Rubinius 1.2.4:

```bash
$ rvm use rbx-1.2.4
Using /home/gaelian/.rvm/gems/rbx-1.2.4-20110705
```

And verify:

```bash
$ ruby -v
rubinius 1.2.4 (1.8.7 release 2011-07-05 JI) [x86_64-unknown-linux-gnu]
```

Nerdgasm imminent.

You can also set the default Ruby for future shell sessions:

```bash
$ rvm use 1.9.3 --default
Using /home/gaelian/.rvm/gems/ruby-1.9.3-p0
```

Now all future shell sessions will use Ruby 1.9.3 supplied via RVM unless you say otherwise.

### Gem sets

The other facet of the hotness that RVM provides is gem sets. When you install a Ruby interpreter, like when we installed Ruby 1.9.3 or Rubinius 1.2.4 just before, RVM also sets up discrete gem set directories for use with your different interpreters. View the gem set directories for Ruby 1.9.3 and Rubinius 1.2.4:

```bash
$ ls -la ~/.rvm/gems
total 28
drwxr-xr-x  7 gaelian gaelian 4096 Oct 31 19:08 .
drwxr-xr-x 24 gaelian gaelian 4096 Oct 31 18:51 ..
drwxr-xr-x  2 gaelian gaelian 4096 Oct 31 18:54 cache
drwxr-xr-x  7 gaelian gaelian 4096 Oct 31 19:07 rbx-1.2.4-20110705
drwxr-xr-x  7 gaelian gaelian 4096 Oct 31 19:08 rbx-1.2.4-20110705@global
drwxr-xr-x  7 gaelian gaelian 4096 Oct 31 18:55 ruby-1.9.3-p0
drwxr-xr-x  7 gaelian gaelian 4096 Oct 31 18:55 ruby-1.9.3-p0@global
```

So we've got Ruby 1.9.3 and Rubinius 1.2.4 installed with each of these interpreters having an un-named gem set and a 'global' gem set. Right now, if you were to install any gems, they'd go into the 'ruby-1.9.3-p0' gem set because Ruby 1.9.3 is the interpreter that you're currently using and we haven't told RVM to use any specific gem set.

Let's switch to the global gem set:

```bash
$ rvm gemset use global
```

Now let's install a few gems:

```bash
$ gem install hpricot rack rake
```

And let's now look in the '~/.rvm/gems/ruby-1.9.3-p0@global/gems' directory:

```bash
$ ls -la ~/.rvm/gems/ruby-1.9.3-p0@global/gems
total 24
drwxr-xr-x 6 gaelian gaelian 4096 Oct 31 19:17 .
drwxr-xr-x 7 gaelian gaelian 4096 Oct 31 18:55 ..
drwxr-xr-x 6 gaelian gaelian 4096 Oct 31 18:55 bundler-1.0.21
drwxr-xr-x 6 gaelian gaelian 4096 Oct 31 19:16 hpricot-0.8.4
drwxr-xr-x 7 gaelian gaelian 4096 Oct 31 19:17 rack-1.3.5
drwxr-xr-x 6 gaelian gaelian 4096 Oct 31 19:17 rake-0.9.2.2
```

Here we see the gems we just installed (bundler is a dependency). But what if we have a particular project that we want to install some gems for and we'd like them to be separate from these other gems we just installed? We can create a new gem set for Ruby 1.9.3 called 'project0' (you can name your gem sets whatever you like, I'm just using 'project0'):

```bash
$ rvm gemset create project0
'project0' gemset created (/home/gaelian/.rvm/gems/ruby-1.9.3-p0@project0).
```

And let's switch to it:

```bash
$ rvm gemset use project0
```

Now let's install another gem into this 'project0' gem set:

```bash
$ gem install rubyzip
```

Let's take a look:

```bash
$ ls -la ~/.rvm/gems/ruby-1.9.3-p0@project0/gems
total 12
drwxr-xr-x 3 gaelian gaelian 4096 Oct 31 19:19 .
drwxr-xr-x 6 gaelian gaelian 4096 Oct 31 19:19 ..
drwxr-xr-x 5 gaelian gaelian 4096 Oct 31 19:19 rubyzip-0.9.4
```

Now let's add one more gem set and switch to it:

```bash
$ rvm gemset create project1
'project1' gemset created (/home/gaelian/.rvm/gems/ruby-1.9.3-p0@project1).
$ rvm gemset use project1
```

And we'll install one more gem in the 'project1' gem set:

```bash
$ gem install bcrypt-ruby
```

So far so good. To re-cap, we've got three gem sets that we're interested in for Ruby 1.9.3:

- global - contains bundler, hpricot, rack and rake
- project0 - contains rubyzip
- project1 - contains bcrypt-ruby

Now let's just take a look at what gems we currently have installed:

```bash
$ gem list

*** LOCAL GEMS ***

bcrypt-ruby (3.0.1)
bundler (1.0.21)
hpricot (0.8.4)
rack (1.3.5)
rake (0.9.2.2)
```

What black sorcery is this?! It would appear that we are seeing the gems from the global gem set *plus* bcrypt-ruby from the project1 gem set that we're currently using. This is a feature, not a bug. The global gem set for each interpreter is special in that the gems stored within will be available *along side* those of your current chosen gem set.

To illustrate, let's switch to using the project0 gem set and list our gems:

```bash
$ rvm gemset use project0
$ gem list

*** LOCAL GEMS ***

bundler (1.0.21)
hpricot (0.8.4)
rack (1.3.5)
rake (0.9.2.2)
rubyzip (0.9.4)
```

Now we can see that we're still showing all the gems from the global gem set, but rubyzip (from our current 'project0' gem set) has appeared and bcrypt-ruby (from our deselected 'project1' gem set) is gone.

You can also set the default gem set for use with a particular interpreter:

```bash
$ rvm use 1.9.3@global --default
Using /home/gaelian/.rvm/gems/ruby-1.9.3-p0 with gemset global
```

Now the global gem set will be used by default with the Ruby 1.9.3 interpreter, unless you say otherwise.

Observe:

```bash
$ gem list

*** LOCAL GEMS ***

bundler (1.0.21)
hpricot (0.8.4)
rack (1.3.5)
rake (0.9.2.2)
```

Because we've got the global gem set selected (not 'project0' or 'project1') we're only seeing the gems installed to the global gem set.

Ever wanted to switch between different versions of Rails with relative ease? Using gem sets, now you can.

#### Making things more explicit

One of the problems that I sometimes have is remembering which Ruby interpreter I'm currently using or more often, which gem set I'm currently using with my chosen Ruby interpreter. One way to make things more explicit - should you wish - is to have the current Ruby interpreter and gem set shown at the command prompt.

Add the required line to your ~/.bashrc file:

```bash
$ echo 'PS1="\$(~/.rvm/bin/rvm-prompt) $PS1" # Outputs RVM details at the command prompt' >> ~/.bashrc
```

Close down your terminal session and open a new one. You should now see that your command prompt looks something like:

```bash
ruby-1.9.3-p290@global gaelian@squeeze:~$
```

Your current Ruby interpreter and your current gem set are now showing at the prompt.

#### Conclusion

In this post I've highlighted some of the useful features of RVM. I remember when I first started using RVM, I wasn't really comfortable with it until I had really grokked where all the gem sets were being kept and the idea that I now potentially had more than one Ruby install, each with potentially more than one gem set. That I could now add and remove Ruby interpreters and gem sets at will and so easily was a new paradigm. I was so used to the Ruby interpreter being this relatively entrenched piece of software sitting under everything else I was doing and it wasn't super easy to upgrade or run in parallel with other Ruby versions/implementations. Similarly, gems were a bit of a pain to manage across different projects when all I had was a single gem directory.

All this has changed. Ruby development is now far more modular and flexible thanks to RVM.

Should you wish for the full lowdown on RVM, the [official RVM site](http://beginrescueend.com/) should probably be your first port of call, they've got some pretty good documentation over there, which I must credit for assisting me to write this post.

**UPDATE:** Thanks to reader fabio who provided a correction (it's the 'subversion' package, not the 'svn' package) and created [a quick install script based on this post](https://github.com/fabiop/ruby-1.9.3-on-debian-squeeze]).

[^1]: The full list of dependences can be found on the RVM website, but for convenience they are: bash (>= 3.2), awk, sed, grep, which, ls, cp, tar, curl, gunzip, bunzip2, git (for updating RVM itself, and for installing '-head' versions of rubies) and subversion (for installing '-head' rubies)
