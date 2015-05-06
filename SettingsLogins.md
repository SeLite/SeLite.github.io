---
title: Settings Logins
layout: default
---


# Summary #
It's a bad practice to hard-code test logins in your test scripts or components. SeLite helps you to make it flexible, but it can't do all the work for you.

# Firefox password store #
If you use Firefox password store and you've saved the passwords (for the tested website), your test can access those passwords via _SeLiteMisc.loginManagerPassword()_. You just need to configure/store the username(s) - see options below.

Beware that Firefox recovers the passwords as plain text. So they are only as safe as your home folder (which contains your Firefox profile).

# Short fixed-length set of roles or placeholders for users #
If you have a short set of roles or username placeholders, of fixed length, which you want to be configurable, you could have a separate Settings field for each and configure the username through it. If you don't use Firefox password store, then store the passwords in another set of fields - one for each such role.

So, for each role or username placeholder, you create one or two instances of _SeLiteSettings.Field.String_.

# Credentials hard-coded in Settings module configuration #
If you just want to configure one choice from a fixed list of credentials, then use single-valued _SeLiteSettings.Field.Choice.String_. The keys can be passwords and the values are usernames (displayed in [SettingsInterface](SettingsInterface)). (The passwords must be different across those usernames). If using Firefox password store, then _Field.Choice.String_ keys and values can be the same - just usernames.

# Roles hard-coded in Settings module configuration #
This uses _SeLiteSettings.Field.FixedMap_, which provides multivalued freetype fields with a fixed keyset. The keys hard-coded in the [Settings](Settings) module definition. The keys serve as symbolic placeholders for roles or usernames. The values can be the actual usernames, entered as freetype. If you don't involve Firefox password store then have a second field with the same keyset, and enter the passwords as values of this field.

## Adding custom keys ##
Frameworks can add keys to fields of class _SeLiteSettings.Field.FixedMap_. One such field is 'extensions.selite-settings.common.roles'. Then they benefit from common functionality like _SeLiteSettings.roleToUser()_. See [InstallFramework](InstallFramework) > [Load and configure the framework](InstallFramework#load-and-configure-the-framework).

## Creating/updating passwords by tests ##
If your tests create/update user passwords, see [TestFrameworks](TestFrameworks) > [Preserving special values in test DB](TestFrameworks#preserving-special-values-in-test-db).

# Out of scope: More flexible or complex fields #
The test scripts contain rolenames or username placeholders - thus you don't want to change them frequently. So there is no need to have the roles or username placeholders editable via GUI interface; it's enough to have them hardcoded in the config field definition.

## Ordered multi-valued freetype fields ##
A more flexible option could be to use two or three multi-valued instances of _SeLiteSettings.Field.String_. The first one would list the role names or username placeholders and the second would contain the actual respective usernames (in the respective order). The third would be for passwords (again, in the respected order) - unless you use Firefox password store. Such paired lists would be flexible, but quite fragile to manage.

Anyway, this wouldn't work out of the box, since multivalued _SeLiteSettings.Field_ freetype values get sorted automatically. Also, preferences API doesn't guarantee an order of fields. So they would have to be indexed by an auto-generated sequential key.

## Fully flexible dictionary/hash fields ##
SeLite Settings doesn't have any dictionary-like freetype field, which would let you enter pairs of keys (e.g. usernames) and values (e.g. passwords) in the browser. This is not trivial to add to the current implementation, due to its existing complexity and the awkwardness of dealing with Mozilla XUL.