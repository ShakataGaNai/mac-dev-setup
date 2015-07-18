# Mac OS X Dev Setup

This document describes how I set up my development environment on a new OSX machine. It is around set up around [NodeJS](http://nodejs.org/) and general web development. This is heavily modified fork of [/nicolashery/mac-dev-setup](https://github.com/nicolashery/mac-dev-setup), which I used for my original inspiration.

The document assumes you are familiar with a Mac. The steps below were tested on **OS X Yosemite (10.10)**.

Comments? Feel free to yell at me, [@ShakataGaNai](https://twitter.com/ShakataGaNai), on twitter.

## Table of contents
- [System update](#system-update)
- [System preferences](#system-preferences)
- [Google Chrome](#google-chrome)
- [iTerm2](#iterm2)
- [Homebrew](#homebrew)
- [iTerm2](#iterm2)
- [Git](#git)
- [Sublime Text](#sublime-text)
- [MySQL](#mysql)
- [Node.js](#nodejs)
- [JSHint](#jshint)
- [Ruby and RVM](#ruby-and-rvm)
- [LESS](#less)
- [Heroku](#heroku)
- [MongoDB](#mongodb)
- [Redis](#redis)
- [Elasticsearch](#elasticsearch)
- [Projects folder](#projects-folder)
- [Apps](#apps)

## System update

First thing you need to do, on any OS actually, is update the system! For that: **Apple Icon > Software Update...**

## Apps to install
* [Google Chrome](https://www.google.com/intl/en/chrome/browser/)
* [iTerm2](http://www.iterm2.com/)
* Github
* Atom.io
* If you plan on using MySQL later, install either:
 * [Sequel Pro](http://www.sequelpro.com/) -- Free & Very attractive
 * [MySQL Workbench](http://www.mysql.com/products/workbench/) -- Official & Free
* Slack (or your favorite chat system)
*


## Fix OSX

* Show all the things on the desktop
$ defaults write com.apple.finder ShowExternalHardDrivesOnDesktop -bool true
$ defaults write com.apple.finder ShowHardDrivesOnDesktop -bool true
$ defaults write com.apple.finder ShowMountedServersOnDesktop -bool true
$ defaults write com.apple.finder ShowRemovableMediaOnDesktop -bool true

* Fix finder
$ defaults write com.apple.finder AppleShowAllFiles YES
$ defaults write com.apple.finder NewWindowTarget PfHm  
$ defaults write com.apple.finder ShowStatusBar -bool true
$ defaults write com.apple.finder ShowPathbar -bool true
$ defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true


* Show ~/Library
$ chflags nohidden "${HOME}/Library"
$ defaults write NSGlobalDomain com.apple.swipescrolldirection -bool false
* sudo softwareupdate --schedule on
* softwareupdate --list
* softwareupdate --install --all

* Lets not spam the desktop with Screenshots
mkdir ~/Desktop/Screenshots/; defaults write com.apple.screencapture location ~/Desktop/Screenshots; sudo killall SystemUIServer

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://brew.sh/).

### Install

 $ xcode-select --install
 $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

One thing we need to do is tell the system to use programs installed by Hombrew (in `/usr/local/bin`) rather than the OS default if it exists. We do this by adding `/usr/local/bin` to your `$PATH` environment variable:

    $ echo 'export PATH="/usr/local/sbin:/usr/local/bin:$PATH"' >> ~/.bash_profile

Open a new terminal tab with **Cmd+T** (you should also close the old one), then run the following command to make sure everything works:

    $ brew update
    $ brew doctor

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    $ brew install <formula>

To update Homebrew's directory of formulae, run:

    $ brew update



## MySQL

### Install

    $ brew install mysql
    $ mysql.server start
    $ mysqladmin -u root password
    $ mysql -u root -p


## Node.js

Install [Node.js](http://nodejs.org/) with Homebrew:

    $ brew update
    $ brew install node

The formula also installs the [npm](https://npmjs.org/) package manager. However, as suggested by the Homebrew output, we need to add `/usr/local/share/npm/bin` to our path so that npm-installed modules with executables will have them picked up.

To do so, add this line to your `~/.path` file, before the `export PATH` line:

```bash
PATH=/usr/local/share/npm/bin:$PATH
```

Open a new terminal for the `$PATH` changes to take effect.

We also need to tell npm where to find the Xcode Command Line Tools, by running:

    $ sudo xcode-select -switch /usr/bin

(If Xcode Command Line Tools were installed by Xcode, try instead:)

    $ sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer

Node modules are installed locally in the `node_modules` folder of each project by default, but there are at least two that are worth installing globally. Those are [CoffeeScript](http://coffeescript.org/) and [Grunt](http://gruntjs.com/):

    $ npm install -g coffee-script
    $ npm install -g grunt-cli

### Npm usage

To install a package:

    $ npm install <package> # Install locally
    $ npm install -g <package> # Install globally

To install a package and save it in your project's `package.json` file:

    $ npm install <package> --save

To see what's installed:

    $ npm list # Local
    $ npm list -g # Global

To find outdated packages (locally or globally):

    $ npm outdated [-g]

To upgrade all or a particular package:

    $ npm update [<package>]

To uninstall a package:

    $ npm uninstall <package>

##JSHint

JSHint is a JavaScript developer's best friend.

If the extra credit assignment to install Sublime Package Manager was completed, JSHint can be run as part of Sublime Text.

Install JSHint via npm (global install preferred)

    $ npm install -g jshint

Follow additional instructions on the [JSHint Package Manager page](https://sublime.wbond.net/packages/JSHint) or [build it manually](https://github.com/jshint/jshint).


## LESS

CSS preprocessors are becoming quite popular, the most popular processors are [LESS](http://lesscss.org/) and [SASS](http://sass-lang.com). Preprocessing is a lot like compiling code for CSS. It allows you to reuse CSS in many different ways. Let's start out with using LESS as a basic preprocessor, it's used by a lot of popular CSS frameworks like [Bootstrap](http://getbootstrap.com/).

### Install

To install LESS you have to use NPM / Node, which you installed earlier using Homebrew. In the terminal use:

    $ npm install less --global

Note: the `--global` flag is optional but it prevents having to mess around with file paths. You can install without the flag, just know what you're doing.

You can check that it installed properly by using:

    $ lessc --version

This should output some information about the compiler:

    lessc 1.5.1 (LESS Compiler) [JavaScript]

Okay, LESS is installed and running. Great!

### Usage

There's a lot of different ways to use LESS. Generally I use it to compile my stylesheet locally. You can do that by using this command in the terminal:

    $ lessc template.less template.css

The two options are the "input" and "output" files for the compiler. The command looks in the current directory for the LESS stylesheet, compiles it, and outputs it to the second file in the same directory. You can add in paths to keep your project files organized:

    $ lessc less/template.less css/template.css

Read more about LESS on their page here: http://lesscss.org/

## Heroku

[Heroku](http://www.heroku.com/), if you're not already familiar with it, is a [Platform-as-a-Service](http://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) that makes it really easy to deploy your apps online. There are other similar solutions out there, but Heroku was among the first and is currently the most popular. Not only does it make a developer's life easier, but I find that having Heroku deployment in mind when building an app forces you to follow modern app development [best practices](http://www.12factor.net/).

### Install

Assuming that you have an account (sign up if you don't), let's install the [Heroku Client](https://devcenter.heroku.com/articles/using-the-cli) for the command-line. Heroku offers a Mac OS X installer, the [Heroku Toolbelt](https://toolbelt.heroku.com/), that includes the client. But for these kind of tools, I prefer using Homebrew. It allows us to keep better track of what we have installed. Luckily for us, Homebrew includes a `heroku-toolbelt` formula:

    $ brew install heroku-toolbelt

The formula might not have the latest version of the Heroku Client, which is updated pretty often. Let's update it now:

    $ heroku update

Don't be afraid to run `heroku update` every now and then to always have the most recent version.

### Usage

Login to your Heroku account using your email and password:

    $ heroku login

If this is a new account, and since you don't already have a public **SSH key** in your `~/.ssh` directory, it will offer to create one for you. Say yes! It will also upload the key to your Heroku account, which will allow you to deploy apps from this computer.

If it didn't offer create the SSH key for you (i.e. your Heroku account already has SSH keys associated with it), you can do so manually by running:

     $ mkdir ~/.ssh
     $ ssh-keygen -t rsa

Keep the default file name and skip the passphrase by just hitting Enter both times. Then, add the key to your Heroku account:

    $ heroku keys:add

Once the key business is done, you're ready to deploy apps! Heroku has a great [Getting Started](https://devcenter.heroku.com/articles/python) guide, so I'll let you refer to that (the one linked here is for Python, but there is one for every popular language). Heroku uses Git to push code for deployment, so make sure your app is under Git version control. A quick cheat sheet (if you've used Heroku before):

    $ cd myapp/
    $ heroku create myapp
    $ git push heroku master
    $ heroku ps
    $ heroku logs -t

The [Heroku Dev Center](https://devcenter.heroku.com/) is full of great resources, so be sure to check it out!

## MongoDB

[MongoDB](http://www.mongodb.org/) is a popular [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database.

### Install

Installing it is very easy through Homebrew:

    $ brew update
    $ brew install mongo

### Usage

In a terminal, start the MongoDB server:

    $ mongod

In another terminal, connect to the database with the Mongo shell using:

    $ mongo

I'll let you refer to MongoDB's [Getting Started](http://docs.mongodb.org/manual/tutorial/getting-started/) guide for more!

## Redis

[Redis](http://redis.io/) is a blazing fast, in-memory, key-value store, that uses the disk for persistence. It's kind of like a NoSQL database, but there are a lot of [cool things](http://oldblog.antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) that you can do with it that would be hard or inefficient with other database solutions. For example, it's often used as session management or caching by web apps, but it has many other uses.

### Install

To install Redis, use Homebrew:

    $ brew update
    $ brew install redis

### Usage

Start a local Redis server using the default configuration settings with:

    $ redis-server

For advanced usage, you can tweak the configuration file at `/usr/local/etc/redis.conf` (I suggest making a backup first), and use those settings with:

    $ redis-server /usr/local/etc/redis.conf

In another terminal, connect to the server with the Redis command-line interface using:

    $ redis-cli

I'll let you refer to Redis' [documentation](http://redis.io/documentation) or other tutorials for more information.

## Elasticsearch

As it says on the box, [Elasticsearch](http://www.elasticsearch.org/) is a "powerful open source, distributed real-time search and analytics engine". It uses an HTTP REST API, making it really easy to work with from any programming language.

You can use elasticsearch for such cool things as real-time search results, autocomplete, recommendations, machine learning, and more.

### Install

Elasticsearch runs on Java, so check if you have it installed by running:

```bash
java -version
```

If Java isn't installed yet, a window will appear prompting you to install it. Go ahead and click "Install".

Next, install elasticsearch with:

```bash
$ brew install elasticsearch
```

**Note**: Elasticsearch also has a `plugin` program that gets moved to your `PATH`. I find that too generic of a name, so I rename it to `elasticsearch-plugin` by running (will need to do that again if you update elasticsearch):

```bash
$ mv /usr/local/bin/plugin /usr/local/bin/elasticsearch-plugin
```

Below I will use `elasticsearch-plugin`, just replace it with `plugin` if you haven't followed this step.

As you guessed, you can add plugins to elasticsearch. A popular one is [elasticsearch-head](http://mobz.github.io/elasticsearch-head/), which gives you a web interface to the REST API. Install it with:

```bash
$ elasticsearch-plugin --install mobz/elasticsearch-head
```

### Usage

Start a local elasticsearch server with:

```bash
$ elasticsearch -f
```

(The `-f` option tells it to run in the foreground, so you can stop it with `Ctrl+C`.)

Test that the server is working correctly by running:

```bash
$ curl -XGET 'http://localhost:9200/'
```

If you installed the elasticsearch-head plugin, you can visit its interface at `http://localhost:9200/_plugin/head/`.

Elasticsearch's [documentation](http://www.elasticsearch.org/guide/) is more of a reference. To get started, I suggest reading some of the blog posts linked on this [StackOverflow answer](http://stackoverflow.com/questions/11593035/beginners-guide-to-elasticsearch/11767610#11767610).

## Projects folder

This really depends on how you want to organize your files, but I like to put all my version-controlled projects in `~/Projects`. Other documents I may have, or things not yet under version control, I like to put in `~/Dropbox` (if you have Dropbox installed), or `~/Documents`.

## Apps

Here is a quick list of some apps I use, and that you might find useful as well:

- [Dropbox](https://www.dropbox.com/): File syncing to the cloud. I put all my documents in Dropbox. It syncs them to all my devices (laptop, mobile, tablet), and serves as a backup as well! **(Free for 2GB)**
- [Google Drive](https://drive.google.com/): File syncing to the cloud too! I use Google Docs a lot to collaborate with others (edit a document with multiple people in real-time!), and sometimes upload other non-Google documents (pictures, etc.), so the app comes in handy for that. **(Free for 5GB)**
- [1Password](https://agilebits.com/onepassword): Allows you to securely store your login and passwords. Even if you only use a few different passwords (they say you shouldn't!), this is really handy to keep track of all the accounts you sign up for! Also, they have a mobile app so you always have all your passwords with you (syncs with Dropbox). A little pricey though. There are free alternatives. **($50 for Mac app, $18 for iOS app)**
- [Marked](http://markedapp.com/): As a developer, most of the stuff you write ends up being in [Markdown](http://daringfireball.net/projects/markdown/). In fact, this `README.md` file (possibly the most important file of a GitHub repo) is indeed in Markdown, written in Sublime Text, and I use Marked to preview the results everytime I save. **($4)**
- [Path Finder](http://cocoatech.com/pathfinder/): I love OSX, it's Unix so great for developers, and all of it just works and looks pretty! Only thing I "miss" from Windows (OMG what did he say?), is a decent file explorer. I think Finder is a pain to use. So I gladly paid for this alternative, but I understand others might find it expensive just to not have to use Finder. **($40)**
- [Evernote](https://evernote.com/): If I don't write something down, I'll forget it. As a developer, you learn so many new things every day, and technology keeps changing, it would be insane to want to keep it all in your head. So take notes, sync them to the cloud, and have them on all your devices. To be honest, I switched to
- [Moom](http://manytricks.com/moom/): Don't waste time resizing and moving your windows. Moom makes this very easy. **($10)**
