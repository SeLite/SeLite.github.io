---
layout: default
---

# Get the source #
You need SeLite source to get [AppsFrameworks](AppsFrameworks) and [PackagedScripts](PackagedScripts). You also need it to install development versions of [AddOns](AddOns). Check the source out with a [GIT client](http://git-scm.com/downloads) (which makes receiving updates or managing local modifications easier). Alternatively, download [SeLite as a ZIP](https://github.com/SeLite/SeLite/archive/master.zip) and [SeLite SelBlocks Global as a ZIP](https://github.com/SeLite/SelBlocksGlobal/archive/master.zip).

There are repositories for

* all add-ons (except for [SelBlocksGlobal](SelBlocksGlobal)) and frameworks
* [SelBlocksGlobal](SelBlocksGlobal)
* documentation (you can view it offline as per [AboutDocumentation](AboutDocumentation)).

| **Purpose**                        | **Repository: files and details** | **Download ZIP** | **Clone from GIT** | **Updates (XML feed)** |
|--------------------------------------|---------------------------------------|-----------------------|------|
| SeLite source (except for [SelBlocksGlobal](SelBlocksGlobal)) | [source](https://github.com/SeLite/SeLite) | [Download ZIP](https://github.com/SeLite/SeLite/archive/master.zip) | https://github.com/SeLite/SeLite.git | [Updates (feed)](https://github.com/SeLite/SeLite/commits/master.atom) |
| [SelBlocksGlobal](SelBlocksGlobal) source                | [source](https://github.com/SeLite/SelBlocksGlobal) | [Download ZIP](https://github.com/SeLite/SelBlocksGlobal/archive/master.zip) | https://github.com/SeLite/SelBlocksGlobal.git | [Updates (feed)](https://github.com/SeLite/SeLBlocksGlobal/commits/master.atom) |
| Documentation                | [source](https://github.com/SeLite/SeLite.github.io) | [Download ZIP](https://github.com/SeLite/SeLite.github.io/archive/master.zip) | https://github.com/SeLite/SeLite.github.io.git | [Updates (feed)](https://github.com/SeLite/SeLite.github.io/commits/master.atom) |
{: .table}

# Install add-ons from source #
(If you've already installed any SeLite add-ons from downloads, uninstall them and restart Firefox. Only then apply the next steps.) Run `SeLite\setup_proxies.bat` and `SeLBlocksGlobal\setup_proxy.bat` (or `SeLite/setup_proxies.sh` and `SeLBlocksGlobal/setup_proxy.sh` on Mac OS/Linux). You can provide a Firefox profile name as a parameter, otherwise it uses `default` profile. After setting up proxy files, start Firefox (with that profile).

On Windows (and probably on Mac OS, too):

  * you may need to accept add-ons
  * verify that all SeLite add-ons are enabled at Firefox menu > Tools > Add-ons > Extensions.

Visit Firefox chrome URL _about:config_. Find or create a preference with name `xpinstall.signatures.required` and set it to `false`. (See [MDN Signing and distributing your add-on](https://developer.mozilla.org/en-US/Add-ons/Distribution)). <!-- Also see https://support.mozilla.org/en-US/kb/add-on-signing-in-firefox?as=u&utm_source=inproduct and https://wiki.mozilla.org/Add-ons/Extension_Signing -->

Restart Firefox.

Add-ons set up this way won't receive any updates. You'll need to run `GIT pull` (or download a new `.zip` file and extract it at the same location).

# Install Selenium IDE from source #
You'd need this only for debugging Selenium IDE, or custom add-ons that override it.

If you've already installed Selenium IDE, uninstall it and restart Firefox. Then you have the following two options.

## Download ##
[Download Selenium IDE](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide/) as an `.xpi` file, but don't install it (right click at the link to an `.xpi` file >  'Save Link As...'). Then unzip the `.xpi` file (you may have to rename it to end with `.zip`). It contains several `.xpi` files inside, and you want `selenium-ide.xpi`. Unzip it and point a proxy file to the unzipped folder. All that is done by the following steps for Linux. For Windows or Mac OS see `setup_proxies.bat` or `setup_proxies.sh` above and figure out similar steps to the effect of the following.

```
cd ~/.mozilla/firefox/*.default/extensions
# if you have Selenium IDE installed already from an `.xpi` file, turn Firefox off and:
   rm -rf \{a6fd85ed-e919-4a43-a5af-8da18bda539f\}.xpi

mkdir selenium-ide-X.Y.Z
cd selenium-ide-X.Y.Z
unzip ../selenium-ide-X.Y.Z.zip
pwd > `echo ~/.mozilla/firefox/*.default`/extensions/\{a6fd85ed-e919-4a43-a5af-8da18bda539f\}
```

Restart Firefox.

This way you won't receive any updates. Subscribe to [RSS XML feed](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide/versions/format:rss) of Selenium IDE versions.

## From GitHub ##
```
git clone https://github.com/SeleniumHQ/selenium.git
cd selenium
./go
cd ide/main/src
```

Either `wget https://raw.githubusercontent.com/peter-kehl/selenium/master/ide/main/src/setup_symlinks.sh`, or [download it raw](https://raw.githubusercontent.com/peter-kehl/selenium/master/ide/main/src/setup_symlinks.sh). Alternatively, copy [its code](https://github.com/peter-kehl/selenium/blob/master/ide/main/src/setup_symlinks.sh) and save it as `setup_symlinks.sh`.


```
./setup_symlinks.sh
pwd > `echo ~/.mozilla/firefox/*.default`/extensions/\{a6fd85ed-e919-4a43-a5af-8da18bda539f\}
```

Restart Firefox.

# Debugging Selenium IDE #
For debugging Selenium IDE, apply [DevelopmentTools](DevelopmentTools) > [Debugging](DevelopmentTools#debugging). Then use Firefox > Tools > Web Developer > Browser Toolbox. You need to know a file name. Identify the file using `grep` or some other text search tool. For easier code navigation create a NetBeans project. See [DevelopmentTools](DevelopmentTools) > [NetBeans as a Javascript IDE](DevelopmentTools#netbeans-as-a-javascript-ide).

# Get Firefox Beta/Nightly #
Download Beta version from [Releases](http://ftp.mozilla.org/pub/firefox/releases/) or Nightly version from [Central](http://ftp.mozilla.org/pub/firefox/nightly/latest-mozilla-central/).