---
title: Documentation standard
layout: default
---

# Maintaining the documentation
  * See [Github-flavoured Markdown](https://help.github.com/articles/github-flavored-markdown/) and [Kramdown syntax](http://kramdown.gettalong.org/syntax.html).
  * Write and organise it clearly.
  * Provide examples.
  * Don't have any content before the first heading (<em># Heading... #</em>)
  * Refer to third party documentation if you mention something for the first time (if you have a reference).
  * Try NetBeans. See [DevelopmentTools](DevelopmentTools) > [NetBeans as a Javascript IDE](DevelopmentTools#netbeans-as-a-javascript-ide). Install [Markdown support](https://github.com/madflow/flow-netbeans-markdown). Open the documentation folder as a 'PHP' project.
  * Don't use fixed line length, especially not in documentation source.
    * A good IDE/editor should be capable of wrapping lines. Take benefit from all your available screen area and not worry about line lengths.
    * Reasonably long lines make source search and narrowing down easier. Search results preview containing long enough lines may be enough for you to know that you can skip the occurrence, without opening that line in editor.
    * You need long lines for pre-formatted text, such as code blocks and [Drawn diagrams](#drawn-diagrams).
    * In NetBeans choose menu Tools > Options > Editor > Formatting > Line Wraps: After words.
    * If you need Eclipse, try <http://dev.cdhq.de/eclipse/word-wrap/> (install both components: word wrap and line numbering fix); however, line numbering fix (for Eclipse Kepler) doesn’t work in Eclipse Luna (4.4).
 * format any _chrome://_ URLs in _italic_ but don't make them links. See [AboutDocumentation](AboutDocumentation) > [Firefox _chrome://_ URLs](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui).
 * Don't have blank lines in HTML comments in .md files. Such comments work well online, but not with Markdown Viewer in Firefox (which displays them as content).
 * Don't fully rely on Markdown Viewer (see [AboutDocumentation](AboutDocumentation)).

## Drawn diagrams
Don't use UML tools, as they are more restrictive and less efficient. <a href='Hidden comment: That"s why I didn"t consider using e.g. http://plantuml.sourceforge.net and http://sourceforge.net/projects/plantumlnb'></a>

Draw diagrams in LibreOffice/OpenOffice and save them as .odg. Then Edit > Select All; File > Export (export the selection, rather than the whole drawing area). Export as a .png, not interlaced, with lowest compression. Then commit both .odg and .png to git. TODO: In documentation use URLs to the .png images from git ([selite/diagrams/](https://github.com/selite/selite/tree/master/diagrams) > navigate to .png file > get the URL of 'Raw' link).

### Versions of LibreOffice
  * LibreOffice version that came with CentOS 6.4 didn't export as .png well - the quality was low.
  * CentOS 6.5 has LibreOffice 4.0.4.2, which is better than one from CentOS 6.4, but it still doesn't export very well.
  * LibreOffice 4.1.3.2 that came with Fedora 19 KDE worked well.
  * Fedora 20 KDE has LibreOffice 4.1.4.2, which renders well.

Please

  * follow <http://www.slideshare.net/otikik/how-to-make-awesome-diagrams-for-your-slides>
  * use colours black, Chart 11 for red, Green 4, Chart 12 for blue, Chart 3 for yellow

## Textual object diagrams
[Object Diagrams](https://code.google.com/p/selite/w/list?q=label:ObjectDiagram) draw ownership/reference relationships between objects, fields and functions. They usually don't necessarily describe class inheritance (which is clear from the source<!--TODO: and from Javadoc -->).

Text diagrams don't need to be esthetic, but clear and easy to edit - so you can quickly update them whenever you change the relevant code. They are in plain text, with <strong><, >, ^, v</strong> for connections. The diagrams look like this:

~~~
Example                                     Description

ObjectConstructorName                       Full name of the 'class' - the constructor function of the object.
Below the object constructor name are object fields. Indent them with two spaces.

.fieldName                                  Field with a fixed (known) name. Prefix it with a dot.

[ index description or expression ]         Dynamic field name.

[ ObjectConstructorName.SECRET_FIELD_NAME ] Field with a fixed name, not referred to via a string/numeric literal across the code,
but rather as a value of a defined constant.
~~~

## Headers IDs must be the same as their text
Don't use [Kramdown-specific header IDs](http://kramdown.gettalong.org/syntax.html#specifying-a-header-id), since they don't work in Markdown Viewer in Firefox..

## Generating 'raw' links
GitHub doesn't serve 'Raw' versions of most file types with their MIME, except for images (because of security). So we use [htmlpreview.github.io](http://htmlpreview.github.io) for .html files. For any other files, e.g. .xml, .xsl or .js, use [rawgit.com](http://rawgit.com).

In detail: Use _htmlpreview.github.io_ rather than _rawgit.com_ for .html, because if you pass a generic Github URL (rather than a commit hash or a tag), _htmlpreview.github.io_ fetches the latest commit of that file. So we don't have to update those _htmlpreview.github.io_ links. However, production _cdn.rawgit.com_ caches the files. So if you change e.g. _\<add-on's-name\>/src/chrome/content/reference.xml_ or _extension-sequencer/src/chrome/content/selenese_reference_to_html.xsl_, then update all _cdn.rawgit.com_ URLs to use the new commit hash or tag.

### Updating links to cdn.rawgit.com
Navigate to [github.com/selite/selite](https://github.com/selite/selite) > 'latest commit XYZ...' link near middle top > 'Browse files' button near the right top > locate the file > 'Raw' button. The URL will start with _https://raw.githubusercontent.com/selite/selite/XYZ.../\<add-on's-name\>/src/chrome/..._ Then append the path part of that URL to _https://cdn.rawgit.com/_, i.e. _https://cdn.rawgit.com/selite/selite/XYZ.../\<add-on's-name\>/src/chrome/..._

Apply similar steps to SelBlocksGlobal.