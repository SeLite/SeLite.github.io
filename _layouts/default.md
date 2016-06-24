<!doctype html>
<html xml:lang="en" lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    <!-- Based on http://stackoverflow.com/questions/2268204/favicon-dimensions: 32x32 is the last icon, since Firefox uses the last one. IE requires that I convert .bmp to .ico - it's not enough to rename it, otherwise it won't show up in IE tab. So I used http://image.online-convert.com/convert-to-ico -->
    <link rel="icon" type="image/png" href="favicon-16x16.png" sizes="16x16">
    <link rel="icon" type="image/png" href="favicon-64x64.png" sizes="64x64">
    <link rel="icon" type="image/png" href="favicon-32x32.png" sizes="32x32">
        {% comment %} For highlighting the current menu & current menu item in Bootstrap menu.
           page.url ends with .html (whether on GitHub and in Jekyll), so I treat it.
           The following has to be in three expressions, rather than in one complex expression - otherwise it failed.
         {% endcomment %}
{% if page.rss != null %}
    <link rel="alternate" type="application/rss+xml" title="RSS feed" href="{{ page.rss }}"/>
{% else %}
    <link href="https://github.com/selite/selite.github.io/commits/master.atom" rel="alternate" title="SeLite Documentation updates" type="application/atom+xml">
{% endif %}
{% assign pageNameParts = (page.url | split: '/') %}
{% assign pageNamePartsWithoutSlash = (pageNameParts[1] | split: '.html') %}

{% assign pageName= pageNamePartsWithoutSlash[0] %}
{% assign pageNameInTitleBar= pageName %}

{% comment %} For some reason, pageName=="index" didn't evaluate to true. TODO report {% endcomment %}
{% if pageName == null or pageName contains "index" and "index" contains pageName %}
    {% assign pageName = './' %}
    {% assign pageNameInTitleBar= 'Overview' %}
{% endif %}
    {% capture pageNameInTitleBar %}{{pageNameInTitleBar}}{% if page.title != null %} {{ page.title }}{% endif %}{% endcapture %}
    <title>SeLite > {{pageNameInTitleBar}}</title>
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css">
    <style type="text/css">
        /* Based on http://getbootstrap.com/css/#grid-media-queries - @screen-sm-min */
        @media (max-width: 767px) {
            #toc-desktop-button {display: none;}
        }
        @media (min-width: 768px) {
            #toc-mobile-button {display: none;}
            /* Mobiles browsers don't show favicon and title when scrolling down, so let's show both in Bootstrap toolbar (i.e. shown even when Bootstrap menu is vertically collapsed). However, desktop browsers show favicon and title most of the time; also, we don't want favicon and the title in Bootstrap toolbar on desktops, since then there's less space for the menu to show horizontally, which causes the menu to be split across two lines. */
            #toc-mobile-title {display: none;}
        }
        ul.nav > li > a {
            padding-left: 2px;
            padding-right: 2px;
        }
        .dropdown-menu[data-placement="left"] {
            left: auto;
            right: 0px;
        }
        /* Following, and the respective data-placement="left" in _includes/toc, is from https://github.com/twbs/bootstrap/issues/1411 */
        .navbar .nav>li>.dropdown-menu[data-placement="left"]:before {
            left: auto;
            right: 9px;
        }

        .navbar .nav > li > .dropdown-menu[data-placement="left"]:after {
            left: auto;
            right: 10px;
        }
        
        #markdown-toc-mobile, #markdown-toc-desktop {
            /* From bootstrap.min.css */
            border: 1px solid rgba(0, 0, 0, 0.15);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.176);
        }

        /* Following rules use specific selectors, so that they override bootstrap.min.css. */
         /* Highlight the menu that contains a link to the current page. This has to use custom data-child-urls, since there's no way to make a CSS selector depend on the next element(s) - e.g. the following didn't work:
            .dropdown-menu > li > a[href^="{{ pageName }}"] ::before ul a {color: green;}
         */
         .navbar-default .navbar-nav > li a[data-group-page-names~="{{ pageName }}"] {color: green;}

         /* Highlight the menu item that is the current page. The selector is complex, so that it overrides a rule from bootstrap.min.css when in mobile mode */
        .navbar-default .navbar-nav .open ul.dropdown-menu > li > a[href="{{ pageName }}"] {color: green;}
        /* Only until Jekyll 3 is common. TODO remove then: */
        .navbar-default .navbar-nav .open ul.dropdown-menu > li > a[href="{{ pageName }}.html"] {color: green;}

        /* Override bootstrap.min.css: */
        * code { color: black; }

        #navbar-menu .navbar-nav > li > a {
            padding-bottom: 2px;
            padding-top: 2px;
        }
        body .navbar {
            margin-bottom: 2px;
            min-height: 18px;
        }
        p#toc-mobile-title.navbar-text {
            padding: 0px;
            margin-top: 0px;
            margin-bottom: 0px;
        }
        .navbar
        {
            height:unset !important;
        }
        .navbar-header
        {
            min-height:16px !important;
        }
        button.navbar-toggle {
            padding: 0px;
            margin-top: 0px;
            margin-bottom: 0px;
        }
    </style>
    <script type="text/javascript">
        // Based on https://github.com/twbs/bootstrap/issues/1768:
        function shiftWindow() {
            scrollBy( 0, -1*$("#whole-navbar").height() );
        }
        window.addEventListener("hashchange", shiftWindow);
        
        function load() {
            $('body').css( "padding-top", $("#whole-navbar").height() );
            if (window.location.hash) {
                shiftWindow();
            }
            $( '#markdown-toc' ).appendTo( '#toc-mobile-div' );
            $( '#markdown-toc' ).clone().appendTo( '#toc-desktop-div' );
            
            // After clicking at a link from Table of Contents, collapse the whole expanded menu (on mobile) or collapse TOC (on desktop)
            $( "#toc-mobile-div a" ).click(
                function() {
                    $("#navbar-menu").toggleClass("in");
                }
            );
            $( "#toc-desktop-div a" ).click(
                function() {
                    $("#toc-desktop-div").toggleClass("in");
                }
            );
        }
    </script>
  </head>
  <body onload="load()">
<!-- Based on http://getbootstrap.com/examples/navbar-fixed-top/ -->
<nav class="navbar navbar-default navbar-fixed-top">
  <div class="container-fluid" id="whole-navbar">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar-menu">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <p class="navbar-text" id="toc-mobile-title" data-toggle="collapse" data-target="#navbar-menu"><img alt="SeLite logo" src="favicon-16x16.png" width="16" height="16"/> {{ pageNameInTitleBar }}</p>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="navbar-menu">
      <ul class="nav navbar-nav">
        <li id="toc-mobile-button"><a data-toggle="collapse" href="#toc-mobile-div" class="dropdown-toggle" role="button"><em>This page</em><span class="caret"></span></a>
            <div id="toc-mobile-div" class="collapse">
            </div>
        </li>
        <li id="toc-desktop-button"><a data-toggle="collapse" href="#toc-desktop-div" class="dropdown-toggle" role="button"><em>This page</em><span class="caret"></span></a>
        </li>
        {% include toc asBootstrapMenu="true" %}
      </ul>
    </div><!-- /.navbar-collapse -->
    <div id="toc-desktop-div" class="collapse">
    </div>
  </div><!-- /.container-fluid -->
</nav>
    {{ content }}
<!-- Based on http://getbootstrap.com/components/#navbar -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
    <script type="text/javascript">
        // Following enables both GitHub page-like links (with no .md at the end) and running Jekyll locally. It's because for each page Abc.md Jekyll generates Abc.html. GitHub pages support both URLs Abc and Abc.html; however, we use Abc since it seems clearer and more Markdown-compatible.
        // @TODO Remove once Jekyll 3 is common. See https://github.com/jekyll/jekyll/pull/3452.
        // A reverse of http://stackoverflow.com/questions/15214762/how-can-i-sync-documentation-with-github-pages/16389663#16389663. See also https://github.com/github/pages-gem/issues/69.
        if( location.host!=='selite.github.io' && false) {
            $(function () {
                // Match any local URLs to other files with no extension, i.e. URLS with no protocol, not starting with #, with any directory path (optional), ending with a filename that doesn't contain a dot. This doesn't match './' or URLs ending with '/' (e.g. ones for /index.md or subfolder/index.md), which is OK, since those work well with both GitHub pages and Jekyll.
                var urlWithNoProtocolAndNoExtensionRegex= /^(?!#|[a-z]+:\/\/)(.*\/)?([^/.]+)$/;
                $('a').each(function () {
                    var href = $(this).attr('href');
                    if( urlWithNoProtocolAndNoExtensionRegex.test(href) ) {
                        var indexOfHash= href.indexOf('#');
                        var url= indexOfHash>0
                            ? href.substring(0, indexOfHash)
                            : href;
                        var hashPart= indexOfHash>0
                            ? href.substring(indexOfHash)
                            : '';
                        $(this).attr( 'href', url +'.html' +hashPart );
                    }
                });
            });
        }
        /*
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-62560081-1', 'auto');
  ga('send', 'pageview');
*/
    </script>
  </body>
</html>
