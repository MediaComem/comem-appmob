# COMEM+ Mobile Applications course

The goal of this course is to teach how to develop hybrid mobile applications, i.e. web apps embedded into native apps on multiple platforms.
You will:

* Learn about hybrid mobile application development with [Ionic][ionic] and [Cordova][cordova]
* Learn the basics of [Angular][angular]
* Design and develop a **mobile application** based on the citizen engagement theme introduced in the [previous course][webserv]
* Run the mobile application on your **phone**

This course is a [COMEM+][comem] [web development course][comem-webdev] taught at [HEIG-VD][heig].

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Plan](#plan)
- [What you will need](#what-you-will-need)
- [Evaluation](#evaluation)
- [Useful links](#useful-links)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Plan

* Introduction
  * [Ionic](https://mediacomem.github.io/comem-webdev-docs/2017/subjects/ionic?home=MediaComem%2Fcomem-appmob%23readme)
  * [Angular](https://mediacomem.github.io/comem-webdev-docs/2017/subjects/angular?home=MediaComem%2Fcomem-appmob%23readme)

* Setup
  * Mockups
  * Live setup

* Advanced concepts
  * Angular events
  * [Angular UI router](https://mediacomem.github.io/comem-webdev-docs/2017/subjects/angular-ui-router?home=MediaComem%2Fcomem-appmob%23readme)
  * [JavaScript closures](https://mediacomem.github.io/comem-webdev-docs/2017/subjects/js-closures?home=MediaComem%2Fcomem-appmob%23readme)
  * JavaScript promises
  * Running Ionic apps on a mobile phone
  * Developing Ionic apps with live reload

* Additional tools
  * Geolocation
  * Mapbox
  * Image upload



## What you will need

* A Unix CLI (Git Bash is included with Git on Windows)
* [Git][git-downloads]
* A free [GitHub][github] account
* [Google Chrome][chrome] (recommended, any browser with developer tools will do)
* [Node.js][node] 6+
* [Ionic][ionic-getting-started]
* A free [Mapbox][mapbox] account



## Evaluation

**Features**

* A citizen must be able to register a new account and log in to the app
* A citizen must be able to report an issue at a specific location, optionally with a picture
* A citizen must be able to see what issues there are in a map of the area, and the details of those issues (picture, description, state, etc)
* Citizens and staff members must be able to post comments on issues, and the list of comments for an issue must be visible somewhere in the app
* *Bonus*: A staff member must be able to log in to the app, start working on issues, and resolve or reject them

**Implementation**

* The app must use the Citizen Engagement API
* The app must use geolocation
* The app must use Mapbox (or an equivalent map library)
* The app must use the camera (works only on physical devices)
* The app must be configurable to run in several environments (local computer & mobile phone)

**User guide or presentation page**

You must provide a user guide or presentation page for the app.
You can choose from the following options (one is enough):

* You can present the app in the README of the GitHub repository for the app
* You can upload your app to a store (e.g. Google Play), and write the presentation page as you would for a real app
* You can use any other presentation tool but your user guide or presentation page must be available online

*Bonus*: Make a webcast demonstrating or selling your app



## Useful links

* [Citizen Engagement API documentation](https://mediacomem.github.io/comem-citizen-engagement-api)
* [Ionic setup](https://github.com/MediaComem/comem-citizen-engagement-ionic-setup) ([complete starter project](https://github.com/MediaComem/comem-citizen-engagement-ionic-starter))



[angular]: https://angularjs.org
[chrome]: https://www.google.com/chrome/
[comem]: http://www.heig-vd.ch/comem
[comem-webdev]: https://github.com/MediaComem/comem-webdev
[cordova]: https://cordova.apache.org
[git-downloads]: https://git-scm.com/downloads
[github]: https://github.com
[heig]: http://www.heig-vd.ch
[ionic]: http://ionicframework.com
[ionic-getting-started]: http://ionicframework.com/getting-started/
[mapbox]: https://www.mapbox.com
[node]: https://nodejs.org/
[webserv]: https://github.com/MediaComem/comem-webserv
