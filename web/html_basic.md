## What are HTML tags?
```
HTML tags are used to mark-up HTML elements
HTML tags are surrounded by the two characters < and >
The surrounding characters are called angle brackets
HTML tags normally come in pairs like <b> and </b>
The first tag in a pair is the start tag, the second tag is the end tag
The text between the start and end tags is the element content
HTML tags are not case sensitive, <b> means the same as <B> 
```

## Logical vs. Physical Tags

### Logical Tags
Designed to describe.

```html
<strong></strong>
```

### Physical Tags
Specific instructions.

```html
<b>
<big>
<i>
```

## Nested Tags
Must be closed in the proper order.

```html
<p><b><em>This is NOT the proper way to close nested tags.</p></em></b>
<p><b><em>This is the proper way to close nested tags. </em></b></p>
```

## Why Use Lowercase Tags?
XHTML(the next generation HTML) requires lowercase tags.

Lowercase Tags are recommended.

## Tag Attributes
Tags can have attributes.

```html
<body bgcolor="blud">
<table border="0">
```

## Quote Styles
Double style quotes are the most common, but single style quotes are also allowed.

```html
name = 'Xiaoqiang's "Friend" Wenxue'
```

## Basic HTML Tags List
```html
<html>
<body>
<h1> to <h6> 
<p>
<br>
<hr>
<!--><-->
```

## Logical Tags
| tab | Description |
| :- | :- |
|<abbr\>|Defines an abbreviation|
|<acronym\>| Defines an acronym|
|<address\>|Defines an address element|
|<cite\>|Defines a citation|
|<code\>|Defines ```computer code``` text|
|<blockquote\>|Defines a long quotation|
|<del\>|Defines text|
|<dfn\>|Defines a definition term|
|<em\>|Defines <em>emphasized</em> text|
|<ins\>|Defines <ins>inserted</ins>  text|
|<kbd\>|Defines keyboard text|
|<pre\>|Defines preformatted text|
|<q\>|Defines a short quotation|
|<samp\>|Defines sample computer code|
|<strong\>|Defines <strong>strong</strong> text|
|<var\>|Defines a variable|

## Physical Tags
| tab | Description |
|:-|:-|
|<b\>|Defines <b>bold</b> text|
|<big\>|Defines <big>big</big> text|
|<i\>|Defines <i>italic</i> text|
|<small\>|Defines <small>small</small> text|
|<sup\>|Defines <sup>superscripted</sup> text|
|<sub\>|Defines <sub>subscripted</sub> text|
|<tt\>|Defines teletype text|


## HTML Character Entities
|Result | Description | Entity Name | Entity Number|
|:-:|:-|:-|:-|
|" "| non-breaking space| &nbsp ;| &#160 ;|
| < | less than | &lt ;|&#60 ;|
| > | greater than | &gt ;| &62 ;|
| & | ampersand | &amp ;| &#38 ;|
| " | quotation mark | &quot ;| &$34 ;|
| ' | apostrophe | &apos ;(not IE)| &#39 ;|

## HTML Backgounds 
The bgcolor,backgound and the text attributes in the <body\></body\> tag are deprecated in the latest versions of HTML (HTML 4 and XHTML). CSS should be used instead.

### Bgcolor
```html
<body bgcolor="#000000">
<body bgcolor="rgb(0,0,0)">
<body bgcolor="black"> 
```

### Backgound 
```html
<body background="clouds.gif">
<body background="http://profdevtrain.austincc.edu/html/graphics/clouds.gif"> 
```

## HTML List
### Unordered Lists
```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

### Ordered Lists
```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```

### Definition Lists

```html
<dl>
<dt></dt>
<dd>
</dd>
</dl>
```

## HTML Links
### The Anchor Tag and the Href Attribute
<a href="url">Text to be displayed</a>

### The Target Attribute
<a href=http://www.austincc.edu/ target="_blank">Visit ACC!</a>

### Email Links
<a href="mailto:helpdesk@austincc.edu">Email Help Desk</a>

<a href="mailto:helpdesk@austincc.edu?subject=Email Assistance">Email Help Desk</a>
### The Anchor Tag and the Name Attribute
<a name="top" href="#top">Text to be displayed</a>

## HTML Images
### The Image Tag and the Src Attribute
<img src="graphics/chef.gif">
<img src="../../../other/images/chef.gif">

### The Alt Attribute
The alt attribute tells the reader what he or she is missing on a page if the browser can't load images.

<img src="graphics/chef.git" alt="Smiling Happy Chef">

### Image Dimensions
<img src="graphics/chef.git" width="130" height="101" alt="Smiling Happy Chef">

## Tables
```html
<table border="1" cellspacing="5" cellpadding="10">
    <tr>
        <th>Heading</th>
        <th>Another Heading</th>
    </tr>
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```

### Table Tags
- table : Defines a table
- th : Defines a table header
- tr : Defines a table row
- td : Defines a table cell(data)
- caption : Defines a table caption
- colgroup : Defines groups of table columns
- col : Defines the attribute values for one or more columns in a table

### Table Size
- Table Width
    + table width="500"
    + table width="80"

### HTML Layout Using Tables
