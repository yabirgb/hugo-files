+++
title = 'Leaving google'
date = '2018-02-17'
author = 'Yábir García'
tags = ['tech', 'google']
+++

Recently I have tried, as the title says, scape from google as much I
can. The first thing to care is my smartphone.

I use a _OnePlus One_, so it has android inside. Since a year or so
I'm using [LineageOS](https://lineageos.org/) with a custom ROM,
[TugaPower](http://tugapower.tk/). All the time I have been using
Gapps but now I updated to the Android 8.1 beta of my ROM and
installed [microG](https://microg.org/).

MicroG is a project aiming to replace the Google's code in Android
smartphones with a free software clone of the libraries from Google
used by apps. You can use your device without _microG_ but things like
push notifications won't work.

_TugaPower_ is shiped without Google's software so you are free to
install whatever you want (it is a key point).

To install microG I used [F-Droid](https://f-droid.org/) as is
described in the [download section](https://microg.org/download.html).

First you need to download _F-Droid_ and after that add to the
repositories _microG_ in the setting of the app once installed. After
that I searched `microg` and installed `microG Services Core` and
`microG Services Framework Proxy`. With that all is set-up and ready.

There are some apps that I use and are not available in the _F-Droid_
market. To solve that I installed [_Yalp
Store_](https://f-droid.org/en/packages/com.github.yeriomin.yalpstore/)
from _F-Droid_.

_Yalp_ makes something like a fake id and downloads the APKs from the
Google's Play Store. 

The APPs that I have installed from _F-Droid_ are:

* Amaze - Manage files
* Camera Roll - Gallery app
* K-9 Mail - Email client
* KeePass DX - KeePass client for Android
* NewPipe - A youtube interface
* Simple Contacts - A contacts manager
* [FreeOTP](https://freeotp.github.io/) - Alternative to GAuthenticator
* Gadgetbridge - Use my Pebble
* Yalp Store

From _Yalp Store_ I have downloaded:

* Firefox
* Brave - Another browser based on Chromium but apparently it [cares
  for your privacy](https://brave.com/privacy/).
* ReadEra - Read ebooks
* Tootdon - Mastodon client
* Reddit
* Telegram X

If you are a user of _Telegram_ as well you can install a version from
_F-Droid_ but is the classic app, the `X` version is only available
from the `Play Store`.

The big fail for me is _Google Maps_. It offers a great service and
has a lot of
information. [OpenStreetMap](https://www.openstreetmap.org/) is cool
but doesn't work well for me. A possible alternative is [Here
Maps](https://www.here.com/en) but is not as complete as _GMaps_.
Also even when Google Maps works good it takes more time to know where
you are.

As alternative to _Gmail_ I use [ProtonMail](https://protonmail.com/).

For my contact list I exported my complete list from gmail (the list
that is always there when you sync your android with google) and used
`Simple Contacts` to import the file in my phone. Now I can also
export it so I don't need Google to sync my list.

Also as a developer I have removed _Google Analytics_ from my projects.
