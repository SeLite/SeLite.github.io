---
layout: default
---
{% include links %}

The example [script] logs in as an admin and creates a user with random credentials and settings. It logs in as that user and posts and validates random content. It stores user settings preferences (e.g. preference for WYSIWYG) in the [script DB]. It validates that the application honours those settings.

# Install Serendipity #
This worked on Fedora 20. I've cloned the github repository up to commit e12b998bf7 into `/var/www/html/serendipity` (git clone https://github.com/s9y/Serendipity.git). Install Serendipity as per its [documentation](http://www.s9y.org/36.html).

The easiest way is using Serendipity with SQLite. If you choose SQLite and you don't provide any db-related fields, the installer creates a .db file under webroot (e.g. _`serendipity_a2a42d7580267f6dcf1a3fd779040498.db`_) and puts that filename (excluding .db extension) in field dbName in `serendipity_config_local.inc.php`. The default `dbPrefix` is `serendipity_`.

# Install SeLite Serendipity framework #
Apply [InstallFramework](InstallFramework). The framework and [scripts][script] are in [serendipity](https://code.google.com/p/selite/source/browse/serendipity) folder.

## Scope ##
The framework tries not to rely on

  * text constants,
  * CSS classes or
  * exact HTML structure.
Therefore it should work with embedded configurations or various languages. It could be used with custom templates (unless they differ a lot). It uses
  * `name` attribute of inputs
  * URLs of links
  * some element IDs (e.g. `serendipity_iframe`)

# Maintaining the framework #
  * Keep it honour configuration of Serendipity installation - as per [http://www.s9y.org/66.html](http://www.s9y.org/66.html) > Paths, Permalinks, General settings, Appearance and options.
  * If you need to target elements by text, see [http://www.s9y.org/137.html](http://www.s9y.org/137.html) > Internationalization

The framework honours Serendipity settings (global and user-specific - stored in `serendipity_config`). @TODO: Make it update the [script DB] to reflect change of settings. Check whether the config pages can be customised via templates or not.

## Boolean fields ##
Serendipity uses string literals `'true'` and `'false'` for some fields which have SQLite column type `boolean` (e.g. `allow_comments` and `moderate_comments` in `serendipity_entries`. However, do not use Javascript boolean literals `true` or `false`: see [SQLiteSpecifics](SQLiteSpecifics) > [No literals true and false](SQLiteSpecifics#no-literals-true-and-false).

## webRoot ##
The framework doesn't use SeLite Settings configuration field `extensions.selite-settings.common.webRoot` (and it removes this field). It uses Serendipity field 'URL to blog' stored in `serendipity_config` with `name='defaultBaseURL'`, as well as configuration for `name='serendipityPath'` and `name='indexFile'`.

## multihosting ##
The framework doesn't autodetect the paths based on HTTP host (it would be complicated, because it would also need to pick a [script DB] relevant to the host etc.). If you'd like the script to work with auto-detected multiple hosts, probably the easiest way is to have one [suite] per host. Have the [suites][suite] in separate folders and associate each folder with a separate [Settings](Settings) configuration via [SettingsManifests](SettingsManifests). Those suites can share same [cases][case]. Alternatively, have the same suites symlinked from multiple directories (at the same depth), but each folder having a separate configuration.