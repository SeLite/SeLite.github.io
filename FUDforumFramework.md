---
title: (for FUDforum 3)
layout: default
---
{% include links %}

The example script logs in as an admin and creates a user with random credentials. It logs in as that user and posts and validates random content.

# Install FUDforum 3 #
I had some sessions issues with `FUDforum_3.0.6RC2.zip`. Therefore I've installed it from SVN, trunk (http://sourceforge.net/p/fudforum/code/HEAD/tree/trunk/) revision 5765. See [Installation > Create your own install script](http://cvs.prohost.org/index.php?title=Installation#Create_your_own_install_script). In addition to that, you can try [Installation > Command line installation](http://cvs.prohost.org/index.php?title=Installation#Command_line_installation).

SeLite can't solve the captcha (it would be a weak captcha otherwise). Therefore select Administration > Global Settings Manager > Disable Captcha Test.

# Install SeLite #
Install recent [Firefox](http://www.mozilla.org) and all SeLite [Components](Components).

# Install SeLite FUDforum framework #
Apply [InstallFramework](InstallFramework). The framework and [scripts][script] are in [fudforum](https://github.com/SeLite/SeLite/tree/master/fudforum) folder. Note that `appDB` filename ends with `.db.php` (e.g. `/var/www/fudforum-data/forum.db.php`).

# DB schema #
When extending DB table definitions in `fudforum/fudforum-framework.js` see e.g. [http://localhost/fudforum](http://localhost/fudforum) > Administration (near top right) > SQL (near top left) > inspect the schema.