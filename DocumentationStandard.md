---
title: Documentation standard
layout: default
---

## Maintaining the documentation
  * See [Github-flavoured Markdown](https://help.github.com/articles/github-flavored-markdown/).
  * Write and organise it clearly.
  * Provide examples.
  * Don't have any content before the first heading (<i># Heading... #</i>)
  * Refer to third party documentation if you mention something for the first time (if you have a reference).
  * Don't use fixed line length, especially not in documentation source. A good IDE/editor should be capable of wrapping lines, so you can benefit from all your available screen area and not worry about line lengths. Also, having longer lines is better when you search for something, as seeing the matches in the search results may give you enough information (e.g. you know you don't need to change that occurrence), which saves you opening that line in editor.
    * Try NetBeans. There choose menu Tools > Options > Editor > Formatting > Line Wraps: After words.
    * If you need Eclipse, try http://dev.cdhq.de/eclipse/word-wrap/ (install both components: word wrap and line numbering fix); however, line numbering fix (for Eclipse Kepler) doesn’t work in Eclipse Luna (4.4).
  * format any _chrome://_ URLs in _italic_ but don't make them links. See [AboutDocumentation](AboutDocumentation) > [Firefox _chrome://_ URLs](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui).
  * Don't have blank lines in HTML comments in .md files. Such comments work well online, but not with Markup Viewer in Firefox (which displays them as content).


### Drawn diagrams ###
Make these nice. Draw them in LibreOffice/OpenOffice and save them as .odg. Then Edit > Select All; File > Export (export the selection, rather than the whole drawing area). Export as a .png, not interlaced, with lowest compression. Then commit both .odg and .png to git. TODO: In wiki use URLs to the .png images from git (https://code.google.com/p/selite/source/browse/diagrams > navigate to .png file > get the url of link 'View raw file').

#### Versions of LibreOffice ####
  * LibreOffice version that came with CentOS 6.4 didn't export as .png well - the quality was low.
  * CentOS 6.5 has LibreOffice 4.0.4.2, which is better than one from CentOS 6.4, but it still doesn't export very well.
  * LibreOffice 4.1.3.2 that came with Fedora 19 KDE worked well.
  * Fedora 20 KDE has LibreOffice 4.1.4.2, which renders well.

Please
  * follow http://www.slideshare.net/otikik/how-to-make-awesome-diagrams-for-your-slides
  * use colours black, Chart 11 for red, Green 4, Chart 12 for blue, Chart 3 for yellow

I think using any UML tools is more restrictive and less efficient. <a href='Hidden comment: That"s why I didn"t consider using e.g. http://plantuml.sourceforge.net and http://sourceforge.net/projects/plantumlnb'></a>

### Textual object diagrams ###
[Object Diagrams](https://code.google.com/p/selite/w/list?q=label:ObjectDiagram) draw ownership/reference relationships between objects, fields and functions. They usually don't necessarily describe class inheritance (which is clear from the source<!--TODO: and from Javadoc -->).

Text diagrams don't need to be esthetic, but clear and easy to edit - so you can quickly update them whenever you change the relevant code. They are in plain text, with <b><, >, ^, v</b> for connections. The diagrams look like this:
~~~
Example                                       Description

ObjectConstructorName                         Full name of the 'class' - the constructor function of the object.
Below the object constructor name are object fields. Indent them with two spaces.

.fieldName                                  Field with a fixed (known) name. Prefix it with a dot.

[ index description or expression ]         Dynamic field name.

[ ObjectConstructorName.SECRET_FIELD_NAME ] Field with a fixed name, not referred to via a string/numeric literal across the code,
but rather as a value of a defined constant.
~~~


### Headers IDs must be the same as their text
Don't use [Kramdown-specific header IDs](http://kramdown.gettalong.org/syntax.html#specifying-a-header-id), since they don't show up well with Markdown Viewer in Firefox.