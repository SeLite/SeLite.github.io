{% include toc %}
<!doctype html>
<html xml:lang="en" lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    <title>{{ page.title }}</title>
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css">
    <style type="text/css">
        /* Based on http://getbootstrap.com/css/#grid-media-queries - @screen-sm-min */
        @media (max-width: 767px) {
            #toc-desktop-button {display: none;}
        }
        @media (min-width: 768px) {
            #toc-mobile-button {display: none;}
            /* Mobiles browsers don't show title when scrolling down, so let's show it in BS menu even when vertically collapsed (where we have nothing else to show). Desktop browsers show title most of the time; also, we don't want the title in BS menu on desktops, since then there's less space for other menu items to show horizontally, which causes them to flow on further lines. */
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
        /* Following, and the respective data-placement="left" in TableOfContents.md, is from https://github.com/twbs/bootstrap/issues/1411 */
        .navbar .nav>li>.dropdown-menu[data-placement="left"]:before {
            left: auto;
            right: 9px;
        }

        .navbar .nav>li>.dropdown-menu[data-placement="left"]:after {
            left: auto;
            right: 10px;
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
      <p class="navbar-text" id="toc-mobile-title" data-toggle="collapse" data-target="#navbar-menu">{{ page.title }}</p>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="navbar-menu">
      <ul class="nav navbar-nav">
        <li id="toc-mobile-button"><a data-toggle="collapse" href="#toc-mobile-div" class="dropdown-toggle" role="button">This page<span class="caret"></span></a>
            <div id="toc-mobile-div" class="collapse">
                {{ toc }}
            </div>
        </li>
        <li id="toc-desktop-button"><a data-toggle="collapse" href="#toc-desktop-div" class="dropdown-toggle" role="button">This page<span class="caret"></span></a>
        </li>
        {% include_relative TableOfContents.md %}
      </ul>
    </div><!-- /.navbar-collapse -->
    <div id="toc-desktop-div" class="collapse">
        {{ toc }}
    </div>
  </div><!-- /.container-fluid -->
</nav>
    {{ content }}
<!-- Based on http://getbootstrap.com/components/#navbar -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js"></script>
    <script type="text/javascript">
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

        // Following enables both GitHub page-like links (with no .md at the end) and running Jekyll locally. It's because for each page Abc.md Jekyll generates Abc.html. GitHub pages support both URLs Abc and Abc.html; however, we use Abc since it seems clearer and more Markdown-compatible.
        // A reverse of http://stackoverflow.com/questions/15214762/how-can-i-sync-documentation-with-github-pages/16389663#16389663. See also https://github.com/github/pages-gem/issues/69.
        if( location.host!=='selite.github.io' ) {
            $(function () {
                // Match any URLs with no protocol, with any directory path (optional), ending with a filename that doesn't contain a dot. This doesn't match './' or URLs ending with '/' (e.g. ones for /index.md or subfolder/index.md), which is OK, since those work well with both GitHub pages and Jekyll.
                var urlWithNoProtocolAndNoExtensionRegex= /^(?![a-z]+:\/\/)(.*\/)?([^/.]+)$/;
                $('a').each(function () {
                    var href = $(this).attr('href');
                    if( urlWithNoProtocolAndNoExtensionRegex.test(href) ) {
                        $(this).attr( 'href', href+'.html' );
                    }
                });
            });
        }
    </script>
  </body>
</html>
