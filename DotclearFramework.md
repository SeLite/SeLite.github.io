---
title: (for Dotclear 2.6.3)
layout: default
---
{% include links %}

# Install Dotclear 2.6.3 #
This is what worked on Fedora 20. The example [script] logs in as an admin and creates a user with random credentials. It logs in as that user and posts and validates random content.

[Download](http://dotclear.org/download) `.tgz` file of Dotclear. Then
<pre>
cd /var/www/html<br>
sudo tar xvzf ~/Downloads/dotclear-2.6.3.tar.gz<br>
<br>
sudo vi dotclear/config.php<br>
# enter path to SQLite file (which doesn’t have to exist, but Apache must be able to create it and to write to it)<br>
<br>
sudo chmod a+rwx public<br>
sudo chmod -R a+rwx cache<br>
</pre>

Configure it at e.g. [http://localhost/dotclear/admin/install/](http://localhost/dotclear/admin/install/).

# Install SeLite Dotclear framework #
Apply [InstallFramework](InstallFramework). The framework and [scripts][script] are in [dotclear](https://code.google.com/p/selite/source/browse/dotclear) folder - e.g. /var/www/html/dotclear/inc/db.sqlite. The default DB prefix is `dc`.