# Mac OS X Dev Setup

This document describes how I set up my development environment on a new OSX machine. It is around set up around [NodeJS](http://nodejs.org/), [MeteorJS](https://www.meteor.com/) and general web development. This is heavily modified fork of [/nicolashery/mac-dev-setup](https://github.com/nicolashery/mac-dev-setup), which I used for my original inspiration.

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

First things first, do the software update's:
```
sudo softwareupdate --schedule on
softwareupdate --list
softwareupdate --install --all
```

## Apps to install
* [Google Chrome](https://www.google.com/intl/en/chrome/browser/)
* [iTerm2](http://www.iterm2.com/)
* [Github Desktop](https://central.github.com/mac/latest)
* [Atom.io](https://atom.io/download/mac)
 * Useful Packages
  * [jshint](https://atom.io/packages/atom-jshint)
  * [wakatime](https://atom.io/packages/wakatime)
* If you plan on using MySQL later, install either:
 * [Sequel Pro](http://www.sequelpro.com/) -- Free & Very attractive
 * [MySQL Workbench](http://www.mysql.com/products/workbench/) -- Official & Free
* [Slack](https://itunes.apple.com/us/app/slack/id803453959?mt=12) (or your favorite chat system)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](https://www.vagrantup.com/downloads.html)
 * [Vagrant Box Search](https://atlas.hashicorp.com/boxes/search?)
 * [Best OSX Box](https://atlas.hashicorp.com/jhcook/boxes/osx-yosemite-10.10) -- `vagrant init jhcook/osx-yosemite-10.10; vagrant up --provider virtualbox`



## Fix OSX

* Show all the things on the desktop
```
defaults write com.apple.finder ShowExternalHardDrivesOnDesktop -bool true
defaults write com.apple.finder ShowHardDrivesOnDesktop -bool true
defaults write com.apple.finder ShowMountedServersOnDesktop -bool true
defaults write com.apple.finder ShowRemovableMediaOnDesktop -bool true
```
* Fix finder
```
defaults write com.apple.finder AppleShowAllFiles YES
defaults write com.apple.finder NewWindowTarget PfHm  
defaults write com.apple.finder ShowStatusBar -bool true
defaults write com.apple.finder ShowPathbar -bool true
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
```
* Show ~/Library
```
chflags nohidden "${HOME}/Library"
defaults write NSGlobalDomain com.apple.swipescrolldirection -bool false
```
* Lets not spam the desktop with Screenshots
```
mkdir ~/Desktop/Screenshots/
defaults write com.apple.screencapture location ~/Desktop/Screenshots
sudo killall SystemUIServer
```

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://brew.sh/).

### Install

```
xcode-select --install
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

One thing we need to do is tell the system to use programs installed by Hombrew (in `/usr/local/bin`) rather than the OS default if it exists. We do this by adding `/usr/local/bin` to your `$PATH` environment variable:

    echo 'export PATH="/usr/local/sbin:/usr/local/bin:$PATH"' >> ~/.bash_profile

Open a new terminal tab with **Cmd+T** (you should also close the old one), then run the following command to make sure everything works:

```
brew update
brew doctor
```

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    $ brew install <formula>

To update Homebrew's directory of formulae, run:

    $ brew update

## Node.js

Install [Node.js](http://nodejs.org/) with Homebrew:

Option A - Install Latest
    $ brew install node

Option B - Install 0.10 (Meteor compatible)
    $brew tap homebrew/versions
    $brew install node010

## Cordova
    $ npm install -g cordova

## Meteor.JS
    $ curl https://install.meteor.com/ | sh

## MySQL

### Install

    brew install mysql
    mysql.server start
    mysqladmin -u root password
    mysql -u root -p

## MongoDB

[MongoDB](http://www.mongodb.org/) is a popular [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database.

    brew update
    brew install mongo

### Usage

In a terminal, start the MongoDB server:

    mongod

In another terminal, connect to the database with the Mongo shell using:

    mongo

I'll let you refer to MongoDB's [Getting Started](http://docs.mongodb.org/manual/tutorial/getting-started/) guide for more!

## Redis

[Redis](http://redis.io/) is a blazing fast, in-memory, key-value store, that uses the disk for persistence. It's kind of like a NoSQL database, but there are a lot of [cool things](http://oldblog.antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) that you can do with it that would be hard or inefficient with other database solutions. For example, it's often used as session management or caching by web apps, but it has many other uses.

### Install

To install Redis, use Homebrew:

    brew update
    brew install redis

### Usage

Start a local Redis server using the default configuration settings with:

    redis-server

For advanced usage, you can tweak the configuration file at `/usr/local/etc/redis.conf` (I suggest making a backup first), and use those settings with:

    redis-server /usr/local/etc/redis.conf

In another terminal, connect to the server with the Redis command-line interface using:

    redis-cli

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


## Apps
- [Dropbox](https://www.dropbox.com/): File syncing to the cloud. I put all my documents in Dropbox. It syncs them to all my devices (laptop, mobile, tablet), and serves as a backup as well! **(Free for 2GB)**
- [Google Drive](https://drive.google.com/): File syncing to the cloud too! I use Google Docs a lot to collaborate with others (edit a document with multiple people in real-time!), and sometimes upload other non-Google documents (pictures, etc.), so the app comes in handy for that. **(Free for 5GB)**
- [1Password](https://agilebits.com/onepassword): Allows you to securely store your login and passwords. Even if you only use a few different passwords (they say you shouldn't!), this is really handy to keep track of all the accounts you sign up for! Also, they have a mobile app so you always have all your passwords with you (syncs with Dropbox). A little pricey though. There are free alternatives. **($50 for Mac app, $18 for iOS app)**
- [Evernote](https://evernote.com/): If I don't write something down, I'll forget it. As a developer, you learn so many new things every day, and technology keeps changing, it would be insane to want to keep it all in your head. So take notes, sync them to the cloud, and have them on all your devices. To be honest, I switched to
- [Moom](http://manytricks.com/moom/): Don't waste time resizing and moving your windows. Moom makes this very easy. **($10)**
