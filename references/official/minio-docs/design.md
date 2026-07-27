---
title: "Design Page — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/design.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "design"
category: "minio"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Design Page — SILO (MinIO Object Storage)"
---
# Design Page

## Overview

This page is designed to assist MinIO UI/UX work on the documentation. Think of
it like a ‘kitchen-sink’ of various objects and headers to more easily see
what everything looks liker right now. If it’s on this page, it exists
somewhere else in the documentation.

If you are not working on MinIO UI/UX, this page has no useful content.
Head back to the [[index|Index]].

## Common Markup

### Text Markup

This is italics text

This is bold text

`This is monospaced text`

[This is hyperlink text](https://min.io)

```
echo "This is a code block. It includes as small copy hover in the top-right."

echo "Syntax highlighting is automatic via pygments."
```

### Containers

This text is in a container. Containers can be helpful for custom rendering
of text. Add a custom class to the container to test the CSS:

```
.. container::
   :class: class-name
```

## Sphinx Design

The following components come from
[sphinx-design](https://github.com/executablebooks/sphinx-design)

### Tabs

MinIO uses the ExecutableBooks
[sphinx-design](https://github.com/executablebooks/sphinx-design)  library for tabs.

Plain Text

This is plain text content

Text and Code

This is plain text content with code:

```
mc admin info ALIAS
```

You can keep tabs in sync. For example, if you have several procedures on a page that have Console and CLI tabs.
Use the :sync: key1 option on teach tab-item.
When someone selects a tab for one or the other, all of the other tabs available on the page with the same key change to that tab, too.

Console

This is plain text content

Command Line

This is plain text content with code:

```
mc admin info ALIAS
```

### Cards

This is the header of the card

Title of the card

This is content inside of the card.

The card can contain varying content.

```
echo "This is some code block"
```

This is the footer of the card

This card is clickable

Clicking this card will take you to [https://min.io](https://min.io)

[](https://min.io)

This card is clickable

Clicking this card will take you to the location of the `objects` reference
anchor.

[[administration/object-management#objects|]]

Card Carousel 1

This is the first item in the carousel

Card Carousel 2

This is the second item in the carousel

Card Carousel 3

This is the third item in the carousel

Card Carousel 4

This is the fourth item in the carousel

### Grids

The first item in the grid

The second item in the grid

The third item in the grid

SubGrid Item 1

SubGrid Item 2

SubGrid Item 3

Card 1

Card 1 content

Card 2

Card 2 content

SubCard 1

SubCard 1 content

SubCard 2

SubCard 2 content

SubCard 3

SubCard 3 content

## Header 1

This is content under a level 1 header. The header includes an
anchor tag for linking. The table of contents for this page is
configured to display up to 2 header levels. The header title should
display in the right hand TOC.

### Header 2

This is content under a level 2 header. The header includes an anchor tag
for linking. The table of contents for this page is configured to display up
to 2 header levels. The header title should display in the right hand TOC

#### Header 3

This is content under a level 3 header. The header includes an anchor tag
for linking. The table of contents for this page is configured to display up to
2 header levels. The header title should not display in the right hand TOC.

##### Header 4

This is content under a level 4 header. The header includes an anchor tag
for linking. The table of contents for this page is configured to display up to
2 header levels. The header title should not display in the right hand TOC.

## Included Header

This is content inside of the above header.
The header decoration must match that of the “parent” document

## Another Header

This is content inside the above header.
This should be the same level as the previous header.

### SubHeader

This is content subsectioned to the previous header.
It should be one level down from the previous header.

## Admonitions

The MinIO documentation uses the following admonition types.
Admonition HTML code resembles the following:

```
<div class="admonition [warning|important|note|custom]>
   <p class="admonition-title"></p>
</div>
```

The additional class is set when defining the admonition and can be
any arbitrary string. Sphinx has defaults around `warning`,
`note`, and `custom`.

### Note Admonition

The note admonition renders as the following:

Note

This text is in the note body. It includes some
`monospaced`, bold, and italics.

This is a [[index|link]] to another page in the documentation.

This is a [link](https://min.io) to an external page.

You can set custom text for the note title:

Custom title with `monospaced` text

This text is in the note body. It includes some
`monospaced`, bold, and italics.

This is a [[index|link]] to another page in the documentation.

This is a [link](https://min.io) to an external page.

To note a version change:

Changed in version RELEASE.2022-07-15T09-20-55Z: `mc license register` replaces the `mc support register` command.

### Important Admonition

The important admonition renders as follows:

Important

This text is in the important body. It includes some
`monospaced`, bold, and italics.

This is a [[index|link]] to another page in the documentation.

This is a [link](https://min.io) to an external page.

You can set custom text for the important title:

This is the important title with `monospaced` text

This text is in the important body. It includes some
`monospaced`, bold, and italics.

This is a [[index|link]] to another page in the documentation.

This is a [link](https://min.io) to an external page.

### Warning Admonition

The warning admonition renders as follows:

Warning

This text is in the warning body. It includes some
`monospaced`, bold, and italics.

This is a [[index|link]] to another page in the documentation.

This is a [link](https://min.io) to an external page.

You can set custom text for the warning title:

This is the warning title with `monospaced` text

This text is in the warning body. It includes some
`monospaced`, bold, and italics.

This is a [[index|link]] to another page in the documentation.

This is a [link](https://min.io) to an external page.

### Generic Admonition

The generic admonition can apply any arbitrary class. This may be
useful if we want to display an admonition using very specific designs.

admonition-title

This text is in the admonition body. It includes some
`monospaced`, bold, and italics.

This is a [[index|link]] to another page in the documentation.

This is a [link](https://min.io) to an external page.

## Lists

### List Table

Sphinx has special markup for producing clean tables, vs ascii-style table
definitions.

The following `.. list-table` has a single header row and multiple columns:

Row Title 1

Row Title 2

Row Title 3

Row Title 4

Column Item 1

Column Item 2

Column Item 3

Column Item 4

Column Item 1

Column Item 2

Column Item 3

Column Item 4

Column Item 1

Column Item 2

Column Item 3

Column Item 4

The following `.. list-table` uses a stub column, where the first column
contains the “header” or title:

Row Title 1

Column Item 1

Column Item 2

Column Item 3

Row Title 2

Column Item 1

Column Item 2

Column Item 3

Row Title 3

Column Item 1

Column Item 2

Column Item 3

Row Title 4

Column Item 1

Column Item 2

Column Item 3

### Bullets and Numbered Lists

This is a bullet list:

Item A

Item B

Item B.1

Item B.2

Item C

Item C.1

Item C.2

Item C.3

Item C.3.1

Item C.3.2

This is a numbered list:

Item A

Item B

Item B.1

Item B.2

Item C

Item C.1

Item C.2

Item C.3

Item C.3.1

Item C.3.2

### Definition Lists

Sphinx markup includes syntax for producing a Description List and
various Description Details. These typically are not anchored, so their
usefulness is somewhat limited. They can be a nice way of creating visually
distinct lists for quick scrolling and view. They are used frequently
in the reference documentation.

Description List Title 1

This is the description body for this title.

Another paragraph in this definition list

Description List Title 2

This is the description body for this title.

Another paragraph in this definition list

Description List Title 3

This is the description body for this title.

Another paragraph in this definition list

## Reference Definition

Sphinx supports creating customized reference-type directives. We use
several throughout the docs. The following section includes some example
definitions. The initial table links to each definition.

[`foo`](#command-foo)

[`foo bar`](#foo.bar)

[`foo bar baz`](#foo.bar.baz)

[`foo`](#data.foo)

[`foo.bar`](#data.foo.bar)

[`--foo`](#cmdoption-foo)

There’s actually a top-level definition here for linking, but not
for display. This is intentional (For now).

foo bar[](#foo.bar)

Used for defining CLI commands.

bin[](#foo.bar.bin)

Used for defining various arguments to a CLI command

baz[](#foo.bar.baz)

Used for defining an option to a CLI command

foo[](#data.foo)

A generic bit of data we can reference.

foo.bar[](#data.foo.bar)

These are nested and linked.

--foo[](#cmdoption-foo)

Used for defining global arguments for CLI commands.

Example:

```
:std:option:`--json <mc.--json>`
```

## Images

[](_images/minio-console1.png)

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).