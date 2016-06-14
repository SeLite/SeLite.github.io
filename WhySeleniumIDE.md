---
layout: default
---
{% include links %}
* TOC
{:toc}

# Similarities between Selenium IDE and WebDriver #
[Selenium](http://seleniumhq.org) - both [Selenium IDE](http://docs.seleniumhq.org/projects/ide/) and [WebDriver](http://seleniumhq.org/projects/webdriver) - automates operation of web browser. It usually doesn't need any modification of the web application. Both alternatives run automation scripts that control the browser (as if it were operated by a human). They

  * navigate through web application in the browser, e.g. they
    * open URLs
    * enter values: type into inputs, choose checkboxes, radio buttons or dropdown lists
    * click at links, buttons and other elements
    * wait until the page refreshes
    * wait for results of background processing (Ajax)
  * identify elements on the page (title, headers, divs, table parts, links, buttons, inputs...) by
    * ID
    * text
    * CSS selectors
    * XPath queries, useful for elements without ID or for dynamic elements. Those can have meaning like
      * 'give me the 2nd cell in the 3rd row of the table with class ABC'
      * 'give me every even link with URL containing /xyz/rst'
    * Javascript
  * validate the pages by checking its URL, title, and elements (their presence or absence, content, style etc.)

# Differences between Selenium IDE and WebDriver #
Following are qualities of Selenium IDE and WebDriver, as relevant to SeLite. (+) and (-) indicate benefits and negatives.

## Selenium IDE ##
  * (+) hands-on, fast life cycle
    * no compilation, no Tomcat restart or the like
    * development, running and sharing of scripts and extensions directly from sources (without packaging)
    * smoother development of extensions with automatic re-loading by [Bootstrap](Bootstrap)
  * (+) easy deployment (no extra DB server, no binary dependencies)
  * (+) no involvement of server-side programming languages
  * (+) convenience of single environment: the browser with Selenium IDE is one place for recording, editing and running the scripts
  * (-) it's for Firefox only
    * (+) In 2017 Firefox extensions will use [the same API](https://developer.mozilla.org/en-US/Add-ons/WebExtensions) as Opera and Chrome.
    * (-) it can't work with other browsers. That may be perceived as a limitation. However, there may not be much benefit in automated testing for multiple browsers, since
      * cross-browser libraries (like JQuery) make applications less browser-dependent
      * there are also application-independent themes or styles, which are tested by their producers
      * differences in browsers mostly affect layout rather than functionality, and
        * defects in presentation are usually minor
        * it's difficult to define how layout should be validated anyway, because
          * applications often allow multiple themes
          * the actual look varies with application state, screen size, device, browser and system
    * (+) there are many third party add-ons, which enhance the language and GUI of Selenium IDE (but not of WebDriver)
    * (+) it uses only Javascript (rather than multiple languages as handled by WebDriver). Using single technology means a bigger group of developers and users. That gives more traction for progress.
    * (+) all components are distributed in source code, rather than in compiled form. Upgrades don't involve any compilation/re-build on user's side.
    * (+) you can package your custom functionality as add-ons, and
      * have them checked for security by Mozilla (whether you make them public or not)
      * easily distribute them (in your private network or publicly), with updates pulled automatically
    * (+) scripts can access privileged Firefox features, like
      * SQLite data storage
        * in a local DB file (under user's control)
        * this allows data separation between the script and the application
        * with object-oriented layer
      * taking screenshots of application screen
      * Login Manager (passwords stored by Firefox), which allows scripts
        * not to keep passwords
        * to be shared with others securely
        * to log on to various websites as one of multiple users, e.g.
          * log on to the application as an admin and change config as needed
          * log on to webmail and check any generated emails

## WebDriver ##
  * (+) supporting various browsers
  * (-) missing some functionality (screenshots) that is only supported in Firefox (see online [Selenium Core Reference](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html) > [Selenium Actions](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html#actions) > `captureEntirePageScreenshot()` or offline at {{chromeUrl}} _chrome://selenium-ide/content/selenium-core/reference.html#actions_ > `captureEntirePageScreenshot()`)
  * (-) less practical for developing, running and distributing the scripts
    * you can import the scripts back from their language to Selenium IDE. However, if you re-edit them, you can't save them in the target language (see [changing formats](http://blog.reallysimplethoughts.com/2011/06/10/does-selenium-ide-v1-0-11-support-changing-formats)). You'd have to maintain both the Selenium IDE scripts (.html) and the generated server-side scripts, and to track any customization, so that you could re-generate them from Selenium IDE.
    * it involves packaging into jar files and restarting Tomcat (or similar)