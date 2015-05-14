---
title: Settings Scope
layout: default
---
{% include links %}

# Level of applying configurations and manifests #
Configurations have effect on [suites][suite], rather than to specific [cases][case]. That's because an automation case can call Selenese function from another [case] (using [SelBlocksGlobal](SelBlocksGlobal)). That would be confusing if those [cases][case] were associated with different configurations.

A manifest affects to any [suite] under its folder subtree, unless overridden by manifest(s) at a more specific (lower) level or by a set (at any level). Set(s) override _values_ manifest(s), so that teams can share default values via _values_ manifests, yet the team members can override them in their own set(s), serving as preferences, without changing any shared manifest(s).

The actual location of [case(s)][case] doesn't matter. They can even be outside of the [suite] folder. So you could run same [case(s)][case] through different suite(s), each with different combination of manifest(s) and set(s).

Manifests work only if the module associates with folders. Otherwise the [script] uses the default set (if the module allows sets) or the only existing set (if the module doesn't allow multiple sets), or a set with a given name (if requested by an explicit API call). See [SettingsAPI](SettingsAPI).

## Multi-valued fields ##
Value(s) of a multi-valued field come from a maximum of one source (set or manifest) - the topmost applicable from the list above. Multi-valued fields don't have the values merged from various sets neither from manifests. It would make them more flexible, but also more confusing and fragile. This also applies to FixedMap fields (see [SettingsFields](SettingsFields)).

# Granularity and overriding through sets and manifests #
The rules of applying values from manifests and configuration sets are, in order of priority:

  * the default set (if any) or any associated configuration sets override _values_ manifests
  * associated sets override the default set
  * more local manifests override less local manifests of the same type (either _values_ or _associations_)
  * all those override default value of the field (from definition of its module)

So, a field will have value(s) based on its topmost occurrence within:

  * most locally associated set
  * less locally associated set
  * ...
  * least locally associated set
  * the default set (if any)
  * most local _values_ manifest
  * less local _values_ manifest
  * ...
  * least local _values_ manifest
  * field default value(s) from module definition (only if the module associates with folders)
<a href='Hidden comment: TODO Check "(only if the module associates with folders)"'></a>

# Caching #
Manifests are cached

  * temporarily when edited through GUI. You just need to refresh the screen in order for a change to apply.
  * fully when used by Selenium [scripts][script] (via `SeLiteData.getStorageFromSettings`). You need to restart Firefox in order for a change to apply.
  * optionally when used by other extensions (the developer can choose whether to cache or not)

# Symlinks (on Mac OS/Unix) #
Symlinks are another way to have variety/separation of configuration sets. If you access a [suite] via a file path which has symlinked non-leaf folder(s), it can be  associated to partially/fully different configuration sets.

<!-- TODO Test how Selenium IDE treats a suite loaded via a path that depends on symlinks - does it use the provided path, or does it resolve it first? If it resolves the path first, then change this paragraph. -->