---
layout: post
title: Samsung Galaxy Nexus 4.1.1 upgrade
---

Samsung Galaxy Nexus 4.1.1 upgrade
==================================

This small tutorial shows how to upgrade your galaxy nexus from *any* version to Jelly Bean 4.1.1 (of course, the very same procedure applies to any given version). Note that this procedure wipes out all your data, this is not a regular upgrade solution.
Steps :

 - If not done already, download + install Android SDK
 - If not done already, download fastboot
 - Download the factory image
  - V4.1.1 Factory Images "yakju" for Galaxy Nexus "maguro" (GSM/HSPA+)
  - from https://developers.google.com/android/nexus/images#yakju
 - Plug mobile to your comptuer
  - Install it all! :)

Isolate it all
--------------
``
mkdir TEMP
cd TEMP
``

Android SDK + Fastboot
----------------------
``
wget http://dl.google.com/android/android-sdk_r20.0.3-linux.tgz
tar cfzv android-sdk_r20.0.3-linux.tgz
android-sdk-linux/tools/android update sdk

wget http://dl.dropbox.com/u/3930957/fastboot # Get fastboot
mv fastboot android-sdk-linux/tools

chmod +x android-sdk-linux/tools/fastboot
chmod +x android-sdk-linux/platform-tools/adb

vi ~/.bashrc
..
export PATH=${PATH}:/PATH/android-sdk-linux/tools
export PATH=${PATH}:/PATH/android-sdk-linux/platform-tools
..
source ~/.bashrc
``

Download Factory image
----------------------
``
cd ..

wget https://dl.google.com/dl/android/aosp/yakju-jro03c-factory-3174c1e5.tgzq
tar cfzv yakju-jro03c-factory-3174c1e5.tgz
cd yakju-jro03c
unzip image-yakju-jro03c.zip
``

Prepare USB driver
----------------------
sudo vi /etc/udev/rules.d/70-android.rules
.. #### file content ####
  # fastboot protocol on maguro (Galaxy Nexus) -> username = whatever you'd like
  SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e30", MODE="0666", OWNER="username"
..
sudo chmod a+rx /etc/udev/rules.d/70-android.rules
sudo reboot
``


Install image to device
-----------------------
``
adb devices # list any devices connected to your computer if any!
adb reboot bootloader # restarts your mobile in bootloader mode == 1) turn off, 2) vol-up+vol-down+power button

cd yakju-jro03c

fastboot devices # list devices connected in bootloader mode
fastboot oem unlock
fastboot reboot-bootloader
fastboot flash bootloader bootloader-maguro-primela03.img
fastboot reboot-bootloader
fastboot flash radio radio-maguro-i9250xxlf1.img
fastboot reboot-bootloader
fastboot flash system system.img
fastboot flash userdata userdata.img
fastboot flash boot boot.img
fastboot flash recovery recovery.img
fastboot erase cache
fastboot reboot

``

# Download Android SDK : http://developer.android.com/sdk/index.html
# Unzip + execute "android update" to update packages (the packages selected by default are fine)


chmod +x android-sdk-linux/tools/fastboot
chmod +x android-sdk-linux/platform-tools/adb

Add these 2 exports to your PATH (eg. in ~/.bashrc)
# Android tools
export PATH=${PATH}:/PATH/android-sdk-linux/tools
export PATH=${PATH}:/PATH/android-sdk-linux/platform-tools

For fastboot to "see" your decive, create this file (change <username> with... your username?):
sudo vi /etc/udev/rules.d/70-android.rules

  # fastboot protocol on maguro (Galaxy Nexus)
  SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e30", MODE="0666", OWNER="<username>"

sudo chmod a+rx /etc/udev/rules.d/70-android.rules
sudo reboot

See what "adb devices" says, if it returns anything, you are good to go! Otherwise... try again! :|

adb devices # list any devices connected to your computer if any!
adb reboot bootloader #restarts your mobile in bootloader mode == 1) turn off, 2) vol-up+vol-down+power button
...
cd /PATH/TO/YAKJU

fastboot devices # list devices connected in bootloader mode
fastboot oem unlock
fastboot reboot-bootloader
fastboot flash bootloader bootloader-maguro-primela03.img
fastboot reboot-bootloader
fastboot flash radio radio-maguro-i9250xxlf1.img
fastboot reboot-bootloader
fastboot flash system system.img
fastboot flash userdata userdata.img
fastboot flash boot boot.img
fastboot flash recovery recovery.img
fastboot erase cache
fastboot reboot


DONE! :)

Dillinger
=========

Dillinger is a cloud-enabled HTML5 Markdown editor.

  - Type some Markdown text in the left window
  - See the HTML in the right
  - Magic

Markdown is a lightweight markup language based on the formatting conventions that people naturally use in email.  As [John Gruber] writes on the [Markdown site] [1]:

> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text your see here is *actually* written in Markdown! To get a feel for Markdown's syntax, type some text into the left window and watch the results in the right.

**Get started by clearing the text with the button 'Clear' in the top navigation.**

Tech
-----------
kjkj
Dillinger uses a number of open source projects to work properly:

* [Ace Editor] - jkjkj  web-based text editor
* [Showdown] - a port of Markdown to JavaScript
* [Twitter Bootstrap] - great UI boilerplate for modern web apps
* [node.js] - evented I/O for the backend
* [Redis] - wickedly fast key-value data store
* [keymaster.js] - awesome keyboard handler lib by [@thomasfuchs]
* [jQuery] - duh


Coming Soon
--------------

**NOTE**: currently the `app.js` file expects a Redis instance to be up and running and available. Dillinger currently uses Redis version **2.4.4**.  You will need to modify the `redis.conf` file if you are going to use an older version of Redis.


Installation
--------------

NOTE: currently the `app.js` file expects a Redis instance to be up and running and available.  It is used for session storage and will be used in the future.

1. Clone the repo
2. `cd dillinger`
3. `npm install -d` (also, if you don't have `smoosh` installed globally execute: `npm install smoosh -g`)
4. `mkdir -p public/files`
5. `mkdir -p public/files/md && mkdir -p public/files/html`
6. Read the Readme file located at `config/README.md` and do what it says.
7. `redis-server config/redis.conf`
8. `node build.js` as this will concat/compress the css and js files.
9. `sudo node app.js`
10. `open http://127.0.0.1`

NOTE: Have a look at the `app.json` file as it has some configuration variables in there as well. You will definitely need to update the `IMAGE_PREFIX_PRODUCTION: "http://cdn.dillinger.io/"` to your own CDN. If you're not using a CDN, set it's path to `/img/` for that is where the images reside locally in the dillinger repo.


*Free Software, Fuck Yeah!*

  [john gruber]: http://daringfireball.net/
  [@thomasfuchs]: http://twitter.com/thomasfuchs
  [1]: http://daringfireball.net/projects/markdown/
  [showdown]: http://www.attacklab.net/
  [ace editor]: http://ace.ajax.org
  [node.js]: http://nodejs.org
  [Twitter Bootstrap]: http://twitter.github.com/bootstrap/
  [keymaster.js]: https://github.com/madrobby/keymaster
  [jQuery]: http://jquery.com
  [Redis]: http://redis.io