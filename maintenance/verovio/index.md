---
title: Creating verovio figures
lang: en
page_language: en
author: Craig Stuart Sapp
creation_date: 10 Mar 2017
last_updated:
ref: maintenance-verovio
search: exclude
tags: [all, maintenance]
vim: ts=3 ft=javascript
verovio: "true"
sidebar: main_sidebar
keywords: maintenance figures verovio music notation
summary: "This page describes how to add verovio-rendered notation figures to documentation pages that have editable Humdrum data alongside the notation."
permalink: /maintenance/verovio/index.html
---

{% include warning.html
	content="You must also activate verovio on the page by including the page header parameter \"`verovio: \"true\"`\" &nbsp;&nbsp; (at the top of the file)."
%}


{% assign specialstart = '{% include verovio.html' %}
{% assign specialend1 = '%' %}
{% assign specialend2 = '}' %}

## Inserting a verovio figure ##

To display Humdrum data alongside the graphical music notation 
that the data represents, include text like this on the webpage:

```liquid
{{ specialstart }}
	source="myhumdrum"
{{ specialend1 }}{{ specialend2 }}
```

The `source` parameter indicates the ID of an element on the page
containing the Humdrum data content.  Also add a `script` element
anywhere else on the page (typically immediately below the verovio.html
include) that contains the Humdrum data to insert into the
Humdrum+notation figure.  The `id` of the script *must* match the one
given in the verovio include with the `source` parameter (and only
one figure on the page can use this particular Humdrum script):

```humdrum
<script type="text/humdrum" id="myhumdrum">
**kern
*M4/4
=1-
4c
4c
4g
4g
=2
4a
4a
2g
=3
4f
4f
4e
4e
=4
4d
4d
2c;
==
*-
</script>
```

Here is resulting the Humdrum+notation figure displayed on the page:

{% include verovio.html
	source="myhumdrum"
%}
<script type="text/humdrum" id="myhumdrum">
**kern
*M4/4
=1-
4c
4c
4g
4g
=2
4a
4a
2g
=3
4f
4f
4e
4e
=4
4d
4d
2c;
==
*-
</script>

You can edit the text in the Humdrum data box above, and the notation should
update as you make changes.  Try adding some sharps or flats to the notes
in the Humdrum data, or changing their octaves. Or try adding this line:
```
!!!RDF**kern: @ = marked note color=hotpink
```
anywhere in the file, and then add `@` characters after some of the note tokens.

## Activating verovio on the page ##

You must also add the line:

```liquid
verovio: true
```

At the top of the Markdown or HTML documentation page, such as [this
one](https://raw.githubusercontent.com/humdrum-tools/vhv-documentation/gh-pages/maintenance/verovio/index.md).  This 
will alos automatically include some [JavaScript support
functions](https://github.com/humdrum-tools/vhv-documentation/tree/master/_includes/verovio_support_functions.html)
that are shared between multiple Humdrum+notation figures on the page.

## Verovio template options ##

The verovio template inserts the contents of
[verovio.html](https://github.com/humdrum-tools/vhv-documentation/tree/master/_includes/verovio.html)
onto the page as it is converted into an HTML webpage, using the
following parameter values to fill in values within the image
template file.  The parameters are in the form:

```liquid
parameter1="value1"
parameter2="value2"
```

Note that there are quotes around the values (and quotes within the value
content must be backquoted: `\"`).  The parameter and the values are separated by
an equals sign, and note that there are no commas between parameters.  Multiple
parameters can be placed on the same line, but for the documentation pages, you
should place a single parameter indented with a tab on each line after the
include line.


Here is a list of the parameters recognized by the verovio template:

### Required parameters ###

source="*id*"
: The **source** parameter's value is an ID of an HTML element on the webpage that stores the Humdrum data.  Below are example container elements, where **source** should be set to *myhumdrum*:

<pre>
&lt;script id="myhumdrum" type="x-application/humdrum"&gt;
**kern
1c
*-
&lt;/script&gt;
</pre>

or

<pre>
&lt;div id="myhumdrum" style="display:none"&gt;
**kern
1c
*-
&lt;/div&gt;
</pre>

In the second case, the `div` element should be hidden on the page,
since the verovio template will copy the Humdrum data to a visible
location on the page by itself (unless the **humdrum-visible** 
parameter is set to *false*).

{% include warning.html
	content="The source *id* must be a unique ID on the HTML page.  If you want to show more than one verovio-generated example on a page, then use a different ID for example, such as \"myhumdrum1\", \"myhumdrum2\", etc.  Note that an HTML ID cannot contain a space and cannot start with a digit and can only contain the additional characters \"-\" and \"_\"."
%}


### Verovio toolkit parameters ###

The following verovio template parameters are passed on to the 
verovio toolkit in order to control layout and options when rendering
graphical notation from the Humdrum data:

font (default `"Leipzig"`)
: This is the musical font to use when creating the music notation.  The
allowable values are `"Leipzig"`, `"Bravura"` or `"Gootville"`.

inputFormat (default `"auto"`)
: This parameter informs the verovio toolkit what type of data is 
being imported.  You can try `"humdrum"` if `"auto"` seems to be causing 
problems.

adjustPageHeight (default `"1"`)
: This parameter is set on by default.  It is used to remove extra blank
space at the end of the notation page.

pageHeight (default `"60000"`)
: This is the height of the notation page in pixels.  Using `"60000"` along
with `adjustPageHeight` set to `"1"` means to place all of the music into
a single SVG image (even of the music is quite long).

pageWidth (default `"1350"`)
: This is the width of the notation page in pixels.  The value 1350 will
make the width of the notation plus the Humdrum data box match the width
of the main text content of the page.

pageWidth (default `"40"`)
: This is the scaling of the music notation in terms of percentage of the
regular scaling (100%). Decrease this value to show more music in the example,
or increase to show the music notation in a larger font.

border (default `"50"`)
: This adds a border around the notation (pixel units).

evenNoteSpacing (default `"0"`)
: Turning this option on by setting it to `"1"` will not add any extra
rhythmic space between melodic notes.  This is used primarily for mensural
music.

page (default `"1"`)
: If the notation renders into more than one page, you can choose which
page to view in the example.  This option may be needed if you shorten
the height of the page.

spacingLinear (default `"0.25"`)
: This parameter controls the compression or expansion of the notation.  Decrease
this value to add more music to each system, or increase it to space the music
more widely.

spacingNonLinear (default `"0.6"`)
: This parameter specifies the ratio of space a rhythm divided by the space
of a rhythm twice as long.  A value of `"1"` means that the spatial width of
an quarter note is 0.6 time longer than that of an eighth note.

spacingStaff (default `"3"`)
: This parameter specifies the number of diatonic stems of vertical space
that is added between staves in a system to increase space between them.

spacingSystem (default `"3"`)
: This parameter specifies the number of diatonic stems of vertical space
that is added between systems to increase space between them. The `spacingStaff`
value will also be added to this value, since the last staff on a system also
adds a `spacingStaff` pad.

### Webpage layout parameters ###

tabsize
: This controls the width of a tab in the display of the Humdrum data.  
The default width is 8 characters but this is too narrow if chords are to
be displayed nicely in multiple columns.  To change to 12 spaces, use `tabsize="12"`.


humdrum-visible="false"
: This parameter will hide the Humdrum text and only show the output graphical
notation:

<div style="margin-top: -40px;">
{% include verovio.html
	source="myhumdrum2"
	pageWidth="1600"
	humdrum-visible="false"
%}
<script type="text/humdrum" id="myhumdrum2">
**kern
*M4/4
=1-
4c
4c
4g
4g
=2
4a
4a
2g
=3
4f
4f
4e
4e
=4
4d
4d
2c;
==
*-
</script>
</div>

humdrum-min-height
: The minimum height of the Humdrum text box.  The Humdrum text box
will normally match the height of the SVG image rendered from verovio.  If
SVG is short, the Humdrum text may be too small for your purposes.  In that
case, increase the minimum height, such as `humdrum-min-height="400px"`.


humdrum-max-width
: the maximum width of the Humdrum text box.



