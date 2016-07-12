---
layout: default
---
{% include links %}
* TOC
{:toc}

# Automatic checks #
[Auto Check](https://addons.mozilla.org/en-US/firefox/addon/selite-auto-check/versions/) ([Components](Components) > [Auto Check](Components#autocheck)) validates the current page after every successful Selenese [command]. (That is not necessarily on every page reload - e.g. it may skip pages that redirect.) You can use some standard checks (with optional configuration) or create custom ones.

## Negative checks ##
Negative checks validate that given selector(s) don't match any elements on the page. That's useful if your web server, framework or programming language report errors/warnings/notices in some kind of fixed format.

There is a standard negative validation for PHP.

### Ignored checks ###
Ignored checks are a special type of negative checks. Once you (or someone else) notice a bug via a negative check, you don't want it to be reported again (until it gets fixed). You can mark such bug as ignored.

Standard PHP detection class supports this (which works with [Xdebug](http://xdebug.org/) or without it). You fill in XPath logical condition(s) that match bugs to be ignored. Alternatively, you can create a custom detection class and provide the logic to filter out ignored bugs.

## Positive checks ##
Positive checks validate that the page contains elements that match given selector(s). However, it can't validate that HTML of a page conforms to HTML standard. That's because Firefox automatically fixes incorrect HTML and it adds missing necessary elements.

## Configuration ##
Configure these checks at {{chromeUrl}} _chrome://selite-settings/content/tree.xul?module=extensions.selite-settings.common_. In order to use Auto Check you must select at least `autoCheckDetector` or `autoCheckDetectorCustom`. Other fields are optional.

  * `autoCheckAssert` - whether an occurrence should trigger an assert failure; by default it triggers a validation failure rather than an assert failure
  * `autoCheckDetector` - choose a standard detection class to use; or set it to `null` or `undefined` if you use a custom class
  * `autoCheckDetectorCustom` - enter a name of the custom detection class (which you must load into [Core scope] e.g. via [Bootstrap](Bootstrap)); used only if `autoCheckDetector` is `null` or `undefined`
  * `autoCheckIgnored` - the format of entries depends on the detection class. Auto Check reports failures unless they match an `autoCheckIgnored` entry. Use `autoCheckIgnored` to match already reported bugs (so that they don't get reported again).
    * for PHP enter XPath logical conditions (not whole XPath expressions). Those would match either PHP warning/notice/error message (description) or a leaf file path (where the failure occurred). The conditions can refer to that text node by '.' (and they must not refer to parent/sibling nodes). Examples are
      * `contains(., 'Undefined variable')`
      * `contains(., 'dbadmin.inc')`
  * `autoCheckRefused` - enter locator(s) to be refused; none of the locator(s) must match any element, otherwise the page fails
  * `autoCheckRequired` - enter locator(s) to be required; each locator must match at least one element, otherwise the page fails

(Those fields are not defined in _chrome://selite-settings/content/common\_settings\_module.js_. Instead, they are added on the fly by _chrome://selite-auto-check/content/SeLiteExtensionSequencerManifest.js_).