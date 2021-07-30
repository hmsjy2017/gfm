<div class="version">

Version 0.29-gfm (2019-04-06)

</div>

<div class="license">

This formal specification is based on the [CommonMark
Spec](http://spec.commonmark.org) by [John
MacFarlane](http://github.com/jgm) and licensed under [![Creative
Commons
BY-SA](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)

</div>

<div id="watermark">

</div>

<div id="TOC">

  - [<span class="number">1</span>Introduction](#introduction)
      - [<span class="number">1.1</span>What is GitHub Flavored
        Markdown?](#what-is-github-flavored-markdown-)
      - [<span class="number">1.2</span>What is
        Markdown?](#what-is-markdown-)
      - [<span class="number">1.3</span>Why is a spec
        needed?](#why-is-a-spec-needed-)
      - [<span class="number">1.4</span>About this
        document](#about-this-document)
  - [<span class="number">2</span>Preliminaries](#preliminaries)
      - [<span class="number">2.1</span>Characters and
        lines](#characters-and-lines)
      - [<span class="number">2.2</span>Tabs](#tabs)
      - [<span class="number">2.3</span>Insecure
        characters](#insecure-characters)
  - [<span class="number">3</span>Blocks and
    inlines](#blocks-and-inlines)
      - [<span class="number">3.1</span>Precedence](#precedence)
      - [<span class="number">3.2</span>Container blocks and leaf
        blocks](#container-blocks-and-leaf-blocks)
  - [<span class="number">4</span>Leaf blocks](#leaf-blocks)
      - [<span class="number">4.1</span>Thematic
        breaks](#thematic-breaks)
      - [<span class="number">4.2</span>ATX headings](#atx-headings)
      - [<span class="number">4.3</span>Setext
        headings](#setext-headings)
      - [<span class="number">4.4</span>Indented code
        blocks](#indented-code-blocks)
      - [<span class="number">4.5</span>Fenced code
        blocks](#fenced-code-blocks)
      - [<span class="number">4.6</span>HTML blocks](#html-blocks)
      - [<span class="number">4.7</span>Link reference
        definitions](#link-reference-definitions)
      - [<span class="number">4.8</span>Paragraphs](#paragraphs)
      - [<span class="number">4.9</span>Blank lines](#blank-lines)
      - <span class="extension">[<span class="number">4.10</span>Tables
        (extension)](#tables-extension-)</span>
  - [<span class="number">5</span>Container blocks](#container-blocks)
      - [<span class="number">5.1</span>Block quotes](#block-quotes)
      - [<span class="number">5.2</span>List items](#list-items)
      - <span class="extension">[<span class="number">5.3</span>Task
        list items (extension)](#task-list-items-extension-)</span>
      - [<span class="number">5.4</span>Lists](#lists)
  - [<span class="number">6</span>Inlines](#inlines)
      - [<span class="number">6.1</span>Backslash
        escapes](#backslash-escapes)
      - [<span class="number">6.2</span>Entity and numeric character
        references](#entity-and-numeric-character-references)
      - [<span class="number">6.3</span>Code spans](#code-spans)
      - [<span class="number">6.4</span>Emphasis and strong
        emphasis](#emphasis-and-strong-emphasis)
      - <span class="extension">[<span class="number">6.5</span>Strikethrough
        (extension)](#strikethrough-extension-)</span>
      - [<span class="number">6.6</span>Links](#links)
      - [<span class="number">6.7</span>Images](#images)
      - [<span class="number">6.8</span>Autolinks](#autolinks)
      - <span class="extension">[<span class="number">6.9</span>Autolinks
        (extension)](#autolinks-extension-)</span>
      - [<span class="number">6.10</span>Raw HTML](#raw-html)
      - <span class="extension">[<span class="number">6.11</span>Disallowed
        Raw HTML (extension)](#disallowed-raw-html-extension-)</span>
      - [<span class="number">6.12</span>Hard line
        breaks](#hard-line-breaks)
      - [<span class="number">6.13</span>Soft line
        breaks](#soft-line-breaks)
      - [<span class="number">6.14</span>Textual
        content](#textual-content)
  - [Appendix: A parsing strategy](#appendix-a-parsing-strategy)
      - [Overview](#overview)
      - [Phase 1: block structure](#phase-1-block-structure)
      - [Phase 2: inline structure](#phase-2-inline-structure)

</div>

# <span class="number">1</span>Introduction

## [](#TOC)<span class="number">1.1</span>What is GitHub Flavored Markdown?

GitHub Flavored Markdown, often shortened as GFM, is the dialect of
Markdown that is currently supported for user content on GitHub.com and
GitHub Enterprise.

This formal specification, based on the CommonMark Spec, defines the
syntax and semantics of this dialect.

GFM is a strict superset of CommonMark. All the features which are
supported in GitHub user content and that are not specified on the
original CommonMark Spec are hence known as **extensions**, and
highlighted as such.

While GFM supports a wide range of inputs, it’s worth noting that
GitHub.com and GitHub Enterprise perform additional post-processing and
sanitization after GFM is converted to HTML to ensure security and
consistency of the website.

## [](#TOC)<span class="number">1.2</span>What is Markdown?

Markdown is a plain text format for writing structured documents, based
on conventions for indicating formatting in email and usenet posts. It
was developed by John Gruber (with help from Aaron Swartz) and released
in 2004 in the form of a [syntax
description](http://daringfireball.net/projects/markdown/syntax) and a
Perl script (`Markdown.pl`) for converting Markdown to HTML. In the next
decade, dozens of implementations were developed in many languages. Some
extended the original Markdown syntax with conventions for footnotes,
tables, and other document elements. Some allowed Markdown documents to
be rendered in formats other than HTML. Websites like Reddit,
StackOverflow, and GitHub had millions of people using Markdown. And
Markdown started to be used beyond the web, to author books, articles,
slide shows, letters, and lecture notes.

What distinguishes Markdown from many other lightweight markup syntaxes,
which are often easier to write, is its readability. As Gruber writes:

> The overriding design goal for Markdown’s formatting syntax is to make
> it as readable as possible. The idea is that a Markdown-formatted
> document should be publishable as-is, as plain text, without looking
> like it’s been marked up with tags or formatting instructions.
> (<http://daringfireball.net/projects/markdown/>)

The point can be illustrated by comparing a sample of
[AsciiDoc](http://www.methods.co.nz/asciidoc/) with an equivalent sample
of Markdown. Here is a sample of AsciiDoc from the AsciiDoc manual:

    1. List item one.
    +
    List item one continued with a second paragraph followed by an
    Indented block.
    +
    .................
    $ ls *.sh
    $ mv *.sh ~/tmp
    .................
    +
    List item continued with a third paragraph.
    
    2. List item two continued with an open block.
    +
    --
    This paragraph is part of the preceding list item.
    
    a. This list is nested and does not require explicit item
    continuation.
    +
    This paragraph is part of the preceding list item.
    
    b. List item b.
    
    This paragraph belongs to item two of the outer list.
    --

And here is the equivalent in Markdown:

    1.  List item one.
    
        List item one continued with a second paragraph followed by an
        Indented block.
    
            $ ls *.sh
            $ mv *.sh ~/tmp
    
        List item continued with a third paragraph.
    
    2.  List item two continued with an open block.
    
        This paragraph is part of the preceding list item.
    
        1. This list is nested and does not require explicit item continuation.
    
           This paragraph is part of the preceding list item.
    
        2. List item b.
    
        This paragraph belongs to item two of the outer list.

The AsciiDoc version is, arguably, easier to write. You don’t need to
worry about indentation. But the Markdown version is much easier to
read. The nesting of list items is apparent to the eye in the source,
not just in the processed document.

## [](#TOC)<span class="number">1.3</span>Why is a spec needed?

John Gruber’s [canonical description of Markdown’s
syntax](http://daringfireball.net/projects/markdown/syntax) does not
specify the syntax unambiguously. Here are some examples of questions it
does not answer:

1.  How much indentation is needed for a sublist? The spec says that
    continuation paragraphs need to be indented four spaces, but is not
    fully explicit about sublists. It is natural to think that they,
    too, must be indented four spaces, but `Markdown.pl` does not
    require that. This is hardly a “corner case,” and divergences
    between implementations on this issue often lead to surprises for
    users in real documents. (See [this comment by John
    Gruber](http://article.gmane.org/gmane.text.markdown.general/1997).)

2.  Is a blank line needed before a block quote or heading? Most
    implementations do not require the blank line. However, this can
    lead to unexpected results in hard-wrapped text, and also to
    ambiguities in parsing (note that some implementations put the
    heading inside the blockquote, while others do not). (John Gruber
    has also spoken [in favor of requiring the blank
    lines](http://article.gmane.org/gmane.text.markdown.general/2146).)

3.  Is a blank line needed before an indented code block? (`Markdown.pl`
    requires it, but this is not mentioned in the documentation, and
    some implementations do not require it.)
    
        paragraph
            code?

4.  What is the exact rule for determining when list items get wrapped
    in `<p>` tags? Can a list be partially “loose” and partially
    “tight”? What should we do with a list like this?
    
        1. one
        
        2. two
        3. three
    
    Or this?
    
        1.  one
            - a
        
            - b
        2.  two
    
    (There are some relevant comments by John Gruber
    [here](http://article.gmane.org/gmane.text.markdown.general/2554).)

5.  Can list markers be indented? Can ordered list markers be
    right-aligned?
    
    ``` 
     8. item 1
     9. item 2
    10. item 2a
    ```

6.  Is this one list with a thematic break in its second item, or two
    lists separated by a thematic break?
    
        * a
        * * * * *
        * b

7.  When list markers change from numbers to bullets, do we have two
    lists or one? (The Markdown syntax description suggests two, but the
    perl scripts and many other implementations produce one.)
    
        1. fee
        2. fie
        -  foe
        -  fum

8.  What are the precedence rules for the markers of inline structure?
    For example, is the following a valid link, or does the code span
    take precedence ?
    
        [a backtick (`)](/url) and [another backtick (`)](/url).

9.  What are the precedence rules for markers of emphasis and strong
    emphasis? For example, how should the following be parsed?
    
        *foo *bar* baz*

10. What are the precedence rules between block-level and inline-level
    structure? For example, how should the following be parsed?
    
        - `a long code span can contain a hyphen like this
          - and it can screw things up`

11. Can list items include section headings? (`Markdown.pl` does not
    allow this, but does allow blockquotes to include headings.)
    
        - # Heading

12. Can list items be empty?
    
        * a
        *
        * b

13. Can link references be defined inside block quotes or list items?
    
        > Blockquote [foo].
        >
        > [foo]: /url

14. If there are multiple definitions for the same reference, which
    takes precedence?
    
        [foo]: /url1
        [foo]: /url2
        
        [foo][]

In the absence of a spec, early implementers consulted `Markdown.pl` to
resolve these ambiguities. But `Markdown.pl` was quite buggy, and gave
manifestly bad results in many cases, so it was not a satisfactory
replacement for a spec.

Because there is no unambiguous spec, implementations have diverged
considerably. As a result, users are often surprised to find that a
document that renders one way on one system (say, a GitHub wiki) renders
differently on another (say, converting to docbook using pandoc). To
make matters worse, because nothing in Markdown counts as a “syntax
error,” the divergence often isn’t discovered right away.

## [](#TOC)<span class="number">1.4</span>About this document

This document attempts to specify Markdown syntax unambiguously. It
contains many examples with side-by-side Markdown and HTML. These are
intended to double as conformance tests. An accompanying script
`spec_tests.py` can be used to run the tests against any Markdown
program:

    python test/spec_tests.py --spec spec.txt --program PROGRAM

Since this document describes how Markdown is to be parsed into an
abstract syntax tree, it would have made sense to use an abstract
representation of the syntax tree instead of HTML. But HTML is capable
of representing the structural distinctions we need to make, and the
choice of HTML for the tests makes it possible to run the tests against
an implementation without writing an abstract syntax tree renderer.

This document is generated from a text file, `spec.txt`, written in
Markdown with a small extension for the side-by-side tests. The script
`tools/makespec.py` can be used to convert `spec.txt` into HTML or
CommonMark (which can then be converted into other formats).

In the examples, the `→` character is used to represent tabs.

# <span class="number">2</span>Preliminaries

## [](#TOC)<span class="number">2.1</span>Characters and lines

Any sequence of [characters](#character) is a valid CommonMark document.

A [character](#character) is a Unicode code point. Although some code
points (for example, combining accents) do not correspond to characters
in an intuitive sense, all code points count as characters for purposes
of this spec.

This spec does not specify an encoding; it thinks of lines as composed
of [characters](#character) rather than bytes. A conforming parser may
be limited to a certain encoding.

A [line](#line) is a sequence of zero or more [characters](#character)
other than newline (`U+000A`) or carriage return (`U+000D`), followed by
a [line ending](#line-ending) or by the end of file.

A [line ending](#line-ending) is a newline (`U+000A`), a carriage return
(`U+000D`) not followed by a newline, or a carriage return and a
following newline.

A line containing no characters, or a line containing only spaces
(`U+0020`) or tabs (`U+0009`), is called a [blank line](#blank-line).

The following definitions of character classes will be used in this
spec:

A [whitespace character](#whitespace-character) is a space (`U+0020`),
tab (`U+0009`), newline (`U+000A`), line tabulation (`U+000B`), form
feed (`U+000C`), or carriage return (`U+000D`).

[Whitespace](#whitespace) is a sequence of one or more [whitespace
characters](#whitespace-character).

A [Unicode whitespace character](#unicode-whitespace-character) is any
code point in the Unicode `Zs` general category, or a tab (`U+0009`),
carriage return (`U+000D`), newline (`U+000A`), or form feed (`U+000C`).

[Unicode whitespace](#unicode-whitespace) is a sequence of one or more
[Unicode whitespace characters](#unicode-whitespace-character).

A [space](#space) is `U+0020`.

A [non-whitespace character](#non-whitespace-character) is any character
that is not a [whitespace character](#whitespace-character).

An [ASCII punctuation character](#ascii-punctuation-character) is `!`,
`"`, `#`, `$`, `%`, `&`, `'`, `(`, `)`, `*`, `+`, `,`, `-`, `.`, `/`
(U+0021–2F), `:`, `;`, `<`, `=`, `>`, `?`, `@` (U+003A–0040), `[`, `\`,
`]`, `^`, `_`, `` ` `` (U+005B–0060), `{`, `|`, `}`, or `~`
(U+007B–007E).

A [punctuation character](#punctuation-character) is an [ASCII
punctuation character](#ascii-punctuation-character) or anything in the
general Unicode categories `Pc`, `Pd`, `Pe`, `Pf`, `Pi`, `Po`, or `Ps`.

## [](#TOC)<span class="number">2.2</span>Tabs

Tabs in lines are not expanded to [spaces](#space). However, in contexts
where whitespace helps to define block structure, tabs behave as if they
were replaced by spaces with a tab stop of 4 characters.

Thus, for example, a tab can be used instead of four spaces in an
indented code block. (Note, however, that internal tabs are passed
through as literal tabs, not expanded to spaces.)

<div id="example-1" class="example">

<div class="examplenum">

[Example 1](#example-1)

</div>

<div class="column">

    →foo→baz→→bim

</div>

<div class="column">

    <pre><code>foo→baz→→bim
    </code></pre>

</div>

</div>

<div id="example-2" class="example">

<div class="examplenum">

[Example 2](#example-2)

</div>

<div class="column">

``` 
  →foo→baz→→bim
```

</div>

<div class="column">

    <pre><code>foo→baz→→bim
    </code></pre>

</div>

</div>

<div id="example-3" class="example">

<div class="examplenum">

[Example 3](#example-3)

</div>

<div class="column">

``` 
    a→a
    ὐ→a
```

</div>

<div class="column">

    <pre><code>a→a
    ὐ→a
    </code></pre>

</div>

</div>

In the following example, a continuation paragraph of a list item is
indented with a tab; this has exactly the same effect as indentation
with four spaces would:

<div id="example-4" class="example">

<div class="examplenum">

[Example 4](#example-4)

</div>

<div class="column">

``` 
  - foo

→bar
```

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <p>bar</p>
    </li>
    </ul>

</div>

</div>

<div id="example-5" class="example">

<div class="examplenum">

[Example 5](#example-5)

</div>

<div class="column">

    - foo
    
    →→bar

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <pre><code>  bar
    </code></pre>
    </li>
    </ul>

</div>

</div>

Normally the `>` that begins a block quote may be followed optionally by
a space, which is not considered part of the content. In the following
case `>` is followed by a tab, which is treated as if it were expanded
into three spaces. Since one of these spaces is considered part of the
delimiter, `foo` is considered to be indented six spaces inside the
block quote context, so we get an indented code block starting with two
spaces.

<div id="example-6" class="example">

<div class="examplenum">

[Example 6](#example-6)

</div>

<div class="column">

    >→→foo

</div>

<div class="column">

    <blockquote>
    <pre><code>  foo
    </code></pre>
    </blockquote>

</div>

</div>

<div id="example-7" class="example">

<div class="examplenum">

[Example 7](#example-7)

</div>

<div class="column">

    -→→foo

</div>

<div class="column">

    <ul>
    <li>
    <pre><code>  foo
    </code></pre>
    </li>
    </ul>

</div>

</div>

<div id="example-8" class="example">

<div class="examplenum">

[Example 8](#example-8)

</div>

<div class="column">

``` 
    foo
→bar
```

</div>

<div class="column">

    <pre><code>foo
    bar
    </code></pre>

</div>

</div>

<div id="example-9" class="example">

<div class="examplenum">

[Example 9](#example-9)

</div>

<div class="column">

``` 
 - foo
   - bar
→ - baz
```

</div>

<div class="column">

    <ul>
    <li>foo
    <ul>
    <li>bar
    <ul>
    <li>baz</li>
    </ul>
    </li>
    </ul>
    </li>
    </ul>

</div>

</div>

<div id="example-10" class="example">

<div class="examplenum">

[Example 10](#example-10)

</div>

<div class="column">

    #→Foo

</div>

<div class="column">

    <h1>Foo</h1>

</div>

</div>

<div id="example-11" class="example">

<div class="examplenum">

[Example 11](#example-11)

</div>

<div class="column">

    *→*→*→

</div>

<div class="column">

    <hr />

</div>

</div>

## [](#TOC)<span class="number">2.3</span>Insecure characters

For security reasons, the Unicode character `U+0000` must be replaced
with the REPLACEMENT CHARACTER (`U+FFFD`).

# <span class="number">3</span>Blocks and inlines

We can think of a document as a sequence of [blocks](#blocks)—structural
elements like paragraphs, block quotations, lists, headings, rules, and
code blocks. Some blocks (like block quotes and list items) contain
other blocks; others (like headings and paragraphs) contain
[inline](#inline) content—text, links, emphasized text, images, code
spans, and so on.

## [](#TOC)<span class="number">3.1</span>Precedence

Indicators of block structure always take precedence over indicators of
inline structure. So, for example, the following is a list with two
items, not a list with one item containing a code span:

<div id="example-12" class="example">

<div class="examplenum">

[Example 12](#example-12)

</div>

<div class="column">

    - `one
    - two`

</div>

<div class="column">

    <ul>
    <li>`one</li>
    <li>two`</li>
    </ul>

</div>

</div>

This means that parsing can proceed in two steps: first, the block
structure of the document can be discerned; second, text lines inside
paragraphs, headings, and other block constructs can be parsed for
inline structure. The second step requires information about link
reference definitions that will be available only at the end of the
first step. Note that the first step requires processing lines in
sequence, but the second can be parallelized, since the inline parsing
of one block element does not affect the inline parsing of any other.

## [](#TOC)<span class="number">3.2</span>Container blocks and leaf blocks

We can divide blocks into two types: [container
blocks](#container-blocks), which can contain other blocks, and [leaf
blocks](#leaf-blocks), which cannot.

# <span class="number">4</span>Leaf blocks

This section describes the different kinds of leaf block that make up a
Markdown document.

## [](#TOC)<span class="number">4.1</span>Thematic breaks

A line consisting of 0-3 spaces of indentation, followed by a sequence
of three or more matching `-`, `_`, or `*` characters, each followed
optionally by any number of spaces or tabs, forms a [thematic
break](#thematic-break).

<div id="example-13" class="example">

<div class="examplenum">

[Example 13](#example-13)

</div>

<div class="column">

    ***
    ---
    ___

</div>

<div class="column">

    <hr />
    <hr />
    <hr />

</div>

</div>

Wrong characters:

<div id="example-14" class="example">

<div class="examplenum">

[Example 14](#example-14)

</div>

<div class="column">

    +++

</div>

<div class="column">

    <p>+++</p>

</div>

</div>

<div id="example-15" class="example">

<div class="examplenum">

[Example 15](#example-15)

</div>

<div class="column">

    ===

</div>

<div class="column">

    <p>===</p>

</div>

</div>

Not enough characters:

<div id="example-16" class="example">

<div class="examplenum">

[Example 16](#example-16)

</div>

<div class="column">

    --
    **
    __

</div>

<div class="column">

    <p>--
    **
    __</p>

</div>

</div>

One to three spaces indent are allowed:

<div id="example-17" class="example">

<div class="examplenum">

[Example 17](#example-17)

</div>

<div class="column">

``` 
 ***
  ***
   ***
```

</div>

<div class="column">

    <hr />
    <hr />
    <hr />

</div>

</div>

Four spaces is too many:

<div id="example-18" class="example">

<div class="examplenum">

[Example 18](#example-18)

</div>

<div class="column">

``` 
    ***
```

</div>

<div class="column">

    <pre><code>***
    </code></pre>

</div>

</div>

<div id="example-19" class="example">

<div class="examplenum">

[Example 19](#example-19)

</div>

<div class="column">

    Foo
        ***

</div>

<div class="column">

    <p>Foo
    ***</p>

</div>

</div>

More than three characters may be used:

<div id="example-20" class="example">

<div class="examplenum">

[Example 20](#example-20)

</div>

<div class="column">

    _____________________________________

</div>

<div class="column">

    <hr />

</div>

</div>

Spaces are allowed between the characters:

<div id="example-21" class="example">

<div class="examplenum">

[Example 21](#example-21)

</div>

<div class="column">

``` 
 - - -
```

</div>

<div class="column">

    <hr />

</div>

</div>

<div id="example-22" class="example">

<div class="examplenum">

[Example 22](#example-22)

</div>

<div class="column">

``` 
 **  * ** * ** * **
```

</div>

<div class="column">

    <hr />

</div>

</div>

<div id="example-23" class="example">

<div class="examplenum">

[Example 23](#example-23)

</div>

<div class="column">

    -     -      -      -

</div>

<div class="column">

    <hr />

</div>

</div>

Spaces are allowed at the end:

<div id="example-24" class="example">

<div class="examplenum">

[Example 24](#example-24)

</div>

<div class="column">

``` 
- - - -    
```

</div>

<div class="column">

    <hr />

</div>

</div>

However, no other characters may occur in the line:

<div id="example-25" class="example">

<div class="examplenum">

[Example 25](#example-25)

</div>

<div class="column">

    _ _ _ _ a
    
    a------
    
    ---a---

</div>

<div class="column">

    <p>_ _ _ _ a</p>
    <p>a------</p>
    <p>---a---</p>

</div>

</div>

It is required that all of the [non-whitespace
characters](#non-whitespace-character) be the same. So, this is not a
thematic break:

<div id="example-26" class="example">

<div class="examplenum">

[Example 26](#example-26)

</div>

<div class="column">

``` 
 *-*
```

</div>

<div class="column">

    <p><em>-</em></p>

</div>

</div>

Thematic breaks do not need blank lines before or after:

<div id="example-27" class="example">

<div class="examplenum">

[Example 27](#example-27)

</div>

<div class="column">

    - foo
    ***
    - bar

</div>

<div class="column">

    <ul>
    <li>foo</li>
    </ul>
    <hr />
    <ul>
    <li>bar</li>
    </ul>

</div>

</div>

Thematic breaks can interrupt a paragraph:

<div id="example-28" class="example">

<div class="examplenum">

[Example 28](#example-28)

</div>

<div class="column">

    Foo
    ***
    bar

</div>

<div class="column">

    <p>Foo</p>
    <hr />
    <p>bar</p>

</div>

</div>

If a line of dashes that meets the above conditions for being a thematic
break could also be interpreted as the underline of a [setext
heading](#setext-heading), the interpretation as a [setext
heading](#setext-heading) takes precedence. Thus, for example, this is a
setext heading, not a paragraph followed by a thematic break:

<div id="example-29" class="example">

<div class="examplenum">

[Example 29](#example-29)

</div>

<div class="column">

    Foo
    ---
    bar

</div>

<div class="column">

    <h2>Foo</h2>
    <p>bar</p>

</div>

</div>

When both a thematic break and a list item are possible interpretations
of a line, the thematic break takes precedence:

<div id="example-30" class="example">

<div class="examplenum">

[Example 30](#example-30)

</div>

<div class="column">

    * Foo
    * * *
    * Bar

</div>

<div class="column">

    <ul>
    <li>Foo</li>
    </ul>
    <hr />
    <ul>
    <li>Bar</li>
    </ul>

</div>

</div>

If you want a thematic break in a list item, use a different bullet:

<div id="example-31" class="example">

<div class="examplenum">

[Example 31](#example-31)

</div>

<div class="column">

    - Foo
    - * * *

</div>

<div class="column">

    <ul>
    <li>Foo</li>
    <li>
    <hr />
    </li>
    </ul>

</div>

</div>

## [](#TOC)<span class="number">4.2</span>ATX headings

An [ATX heading](#atx-heading) consists of a string of characters,
parsed as inline content, between an opening sequence of 1–6 unescaped
`#` characters and an optional closing sequence of any number of
unescaped `#` characters. The opening sequence of `#` characters must be
followed by a [space](#space) or by the end of line. The optional
closing sequence of `#`s must be preceded by a [space](#space) and may
be followed by spaces only. The opening `#` character may be indented
0-3 spaces. The raw contents of the heading are stripped of leading and
trailing spaces before being parsed as inline content. The heading level
is equal to the number of `#` characters in the opening sequence.

Simple headings:

<div id="example-32" class="example">

<div class="examplenum">

[Example 32](#example-32)

</div>

<div class="column">

    # foo
    ## foo
    ### foo
    #### foo
    ##### foo
    ###### foo

</div>

<div class="column">

    <h1>foo</h1>
    <h2>foo</h2>
    <h3>foo</h3>
    <h4>foo</h4>
    <h5>foo</h5>
    <h6>foo</h6>

</div>

</div>

More than six `#` characters is not a heading:

<div id="example-33" class="example">

<div class="examplenum">

[Example 33](#example-33)

</div>

<div class="column">

    ####### foo

</div>

<div class="column">

    <p>####### foo</p>

</div>

</div>

At least one space is required between the `#` characters and the
heading’s contents, unless the heading is empty. Note that many
implementations currently do not require the space. However, the space
was required by the [original ATX
implementation](http://www.aaronsw.com/2002/atx/atx.py), and it helps
prevent things like the following from being parsed as headings:

<div id="example-34" class="example">

<div class="examplenum">

[Example 34](#example-34)

</div>

<div class="column">

    #5 bolt
    
    #hashtag

</div>

<div class="column">

    <p>#5 bolt</p>
    <p>#hashtag</p>

</div>

</div>

This is not a heading, because the first `#` is escaped:

<div id="example-35" class="example">

<div class="examplenum">

[Example 35](#example-35)

</div>

<div class="column">

    \## foo

</div>

<div class="column">

    <p>## foo</p>

</div>

</div>

Contents are parsed as inlines:

<div id="example-36" class="example">

<div class="examplenum">

[Example 36](#example-36)

</div>

<div class="column">

    # foo *bar* \*baz\*

</div>

<div class="column">

    <h1>foo <em>bar</em> *baz*</h1>

</div>

</div>

Leading and trailing [whitespace](#whitespace) is ignored in parsing
inline content:

<div id="example-37" class="example">

<div class="examplenum">

[Example 37](#example-37)

</div>

<div class="column">

``` 
#                  foo                     
```

</div>

<div class="column">

    <h1>foo</h1>

</div>

</div>

One to three spaces indentation are allowed:

<div id="example-38" class="example">

<div class="examplenum">

[Example 38](#example-38)

</div>

<div class="column">

``` 
 ### foo
  ## foo
   # foo
```

</div>

<div class="column">

    <h3>foo</h3>
    <h2>foo</h2>
    <h1>foo</h1>

</div>

</div>

Four spaces are too much:

<div id="example-39" class="example">

<div class="examplenum">

[Example 39](#example-39)

</div>

<div class="column">

``` 
    # foo
```

</div>

<div class="column">

    <pre><code># foo
    </code></pre>

</div>

</div>

<div id="example-40" class="example">

<div class="examplenum">

[Example 40](#example-40)

</div>

<div class="column">

    foo
        # bar

</div>

<div class="column">

    <p>foo
    # bar</p>

</div>

</div>

A closing sequence of `#` characters is optional:

<div id="example-41" class="example">

<div class="examplenum">

[Example 41](#example-41)

</div>

<div class="column">

    ## foo ##
      ###   bar    ###

</div>

<div class="column">

    <h2>foo</h2>
    <h3>bar</h3>

</div>

</div>

It need not be the same length as the opening sequence:

<div id="example-42" class="example">

<div class="examplenum">

[Example 42](#example-42)

</div>

<div class="column">

    # foo ##################################
    ##### foo ##

</div>

<div class="column">

    <h1>foo</h1>
    <h5>foo</h5>

</div>

</div>

Spaces are allowed after the closing sequence:

<div id="example-43" class="example">

<div class="examplenum">

[Example 43](#example-43)

</div>

<div class="column">

``` 
### foo ###     
```

</div>

<div class="column">

    <h3>foo</h3>

</div>

</div>

A sequence of `#` characters with anything but [spaces](#space)
following it is not a closing sequence, but counts as part of the
contents of the heading:

<div id="example-44" class="example">

<div class="examplenum">

[Example 44](#example-44)

</div>

<div class="column">

    ### foo ### b

</div>

<div class="column">

    <h3>foo ### b</h3>

</div>

</div>

The closing sequence must be preceded by a space:

<div id="example-45" class="example">

<div class="examplenum">

[Example 45](#example-45)

</div>

<div class="column">

    # foo#

</div>

<div class="column">

    <h1>foo#</h1>

</div>

</div>

Backslash-escaped `#` characters do not count as part of the closing
sequence:

<div id="example-46" class="example">

<div class="examplenum">

[Example 46](#example-46)

</div>

<div class="column">

    ### foo \###
    ## foo #\##
    # foo \#

</div>

<div class="column">

    <h3>foo ###</h3>
    <h2>foo ###</h2>
    <h1>foo #</h1>

</div>

</div>

ATX headings need not be separated from surrounding content by blank
lines, and they can interrupt paragraphs:

<div id="example-47" class="example">

<div class="examplenum">

[Example 47](#example-47)

</div>

<div class="column">

    ****
    ## foo
    ****

</div>

<div class="column">

    <hr />
    <h2>foo</h2>
    <hr />

</div>

</div>

<div id="example-48" class="example">

<div class="examplenum">

[Example 48](#example-48)

</div>

<div class="column">

    Foo bar
    # baz
    Bar foo

</div>

<div class="column">

    <p>Foo bar</p>
    <h1>baz</h1>
    <p>Bar foo</p>

</div>

</div>

ATX headings can be empty:

<div id="example-49" class="example">

<div class="examplenum">

[Example 49](#example-49)

</div>

<div class="column">

    ## 
    #
    ### ###

</div>

<div class="column">

    <h2></h2>
    <h1></h1>
    <h3></h3>

</div>

</div>

## [](#TOC)<span class="number">4.3</span>Setext headings

A [setext heading](#setext-heading) consists of one or more lines of
text, each containing at least one [non-whitespace
character](#non-whitespace-character), with no more than 3 spaces
indentation, followed by a [setext heading
underline](#setext-heading-underline). The lines of text must be such
that, were they not followed by the setext heading underline, they would
be interpreted as a paragraph: they cannot be interpretable as a [code
fence](#code-fence), [ATX heading](#atx-headings), [block
quote](#block-quotes), [thematic break](#thematic-breaks), [list
item](#list-items), or [HTML block](#html-blocks).

A [setext heading underline](#setext-heading-underline) is a sequence of
`=` characters or a sequence of `-` characters, with no more than 3
spaces indentation and any number of trailing spaces. If a line
containing a single `-` can be interpreted as an empty [list
items](#list-items), it should be interpreted this way and not as a
[setext heading underline](#setext-heading-underline).

The heading is a level 1 heading if `=` characters are used in the
[setext heading underline](#setext-heading-underline), and a level 2
heading if `-` characters are used. The contents of the heading are the
result of parsing the preceding lines of text as CommonMark inline
content.

In general, a setext heading need not be preceded or followed by a blank
line. However, it cannot interrupt a paragraph, so when a setext heading
comes after a paragraph, a blank line is needed between them.

Simple examples:

<div id="example-50" class="example">

<div class="examplenum">

[Example 50](#example-50)

</div>

<div class="column">

    Foo *bar*
    =========
    
    Foo *bar*
    ---------

</div>

<div class="column">

    <h1>Foo <em>bar</em></h1>
    <h2>Foo <em>bar</em></h2>

</div>

</div>

The content of the header may span more than one line:

<div id="example-51" class="example">

<div class="examplenum">

[Example 51](#example-51)

</div>

<div class="column">

    Foo *bar
    baz*
    ====

</div>

<div class="column">

    <h1>Foo <em>bar
    baz</em></h1>

</div>

</div>

The contents are the result of parsing the headings’s raw content as
inlines. The heading’s raw content is formed by concatenating the lines
and removing initial and final [whitespace](#whitespace).

<div id="example-52" class="example">

<div class="examplenum">

[Example 52](#example-52)

</div>

<div class="column">

``` 
  Foo *bar
baz*→
====
```

</div>

<div class="column">

    <h1>Foo <em>bar
    baz</em></h1>

</div>

</div>

The underlining can be any length:

<div id="example-53" class="example">

<div class="examplenum">

[Example 53](#example-53)

</div>

<div class="column">

    Foo
    -------------------------
    
    Foo
    =

</div>

<div class="column">

    <h2>Foo</h2>
    <h1>Foo</h1>

</div>

</div>

The heading content can be indented up to three spaces, and need not
line up with the underlining:

<div id="example-54" class="example">

<div class="examplenum">

[Example 54](#example-54)

</div>

<div class="column">

``` 
   Foo
---

  Foo
-----

  Foo
  ===
```

</div>

<div class="column">

    <h2>Foo</h2>
    <h2>Foo</h2>
    <h1>Foo</h1>

</div>

</div>

Four spaces indent is too much:

<div id="example-55" class="example">

<div class="examplenum">

[Example 55](#example-55)

</div>

<div class="column">

``` 
    Foo
    ---

    Foo
---
```

</div>

<div class="column">

    <pre><code>Foo
    ---
    
    Foo
    </code></pre>
    <hr />

</div>

</div>

The setext heading underline can be indented up to three spaces, and may
have trailing spaces:

<div id="example-56" class="example">

<div class="examplenum">

[Example 56](#example-56)

</div>

<div class="column">

``` 
Foo
   ----      
```

</div>

<div class="column">

    <h2>Foo</h2>

</div>

</div>

Four spaces is too much:

<div id="example-57" class="example">

<div class="examplenum">

[Example 57](#example-57)

</div>

<div class="column">

    Foo
        ---

</div>

<div class="column">

    <p>Foo
    ---</p>

</div>

</div>

The setext heading underline cannot contain internal spaces:

<div id="example-58" class="example">

<div class="examplenum">

[Example 58](#example-58)

</div>

<div class="column">

    Foo
    = =
    
    Foo
    --- -

</div>

<div class="column">

    <p>Foo
    = =</p>
    <p>Foo</p>
    <hr />

</div>

</div>

Trailing spaces in the content line do not cause a line break:

<div id="example-59" class="example">

<div class="examplenum">

[Example 59](#example-59)

</div>

<div class="column">

    Foo  
    -----

</div>

<div class="column">

    <h2>Foo</h2>

</div>

</div>

Nor does a backslash at the end:

<div id="example-60" class="example">

<div class="examplenum">

[Example 60](#example-60)

</div>

<div class="column">

    Foo\
    ----

</div>

<div class="column">

    <h2>Foo\</h2>

</div>

</div>

Since indicators of block structure take precedence over indicators of
inline structure, the following are setext headings:

<div id="example-61" class="example">

<div class="examplenum">

[Example 61](#example-61)

</div>

<div class="column">

    `Foo
    ----
    `
    
    <a title="a lot
    ---
    of dashes"/>

</div>

<div class="column">

    <h2>`Foo</h2>
    <p>`</p>
    <h2>&lt;a title=&quot;a lot</h2>
    <p>of dashes&quot;/&gt;</p>

</div>

</div>

The setext heading underline cannot be a [lazy continuation
line](#lazy-continuation-line) in a list item or block quote:

<div id="example-62" class="example">

<div class="examplenum">

[Example 62](#example-62)

</div>

<div class="column">

    > Foo
    ---

</div>

<div class="column">

    <blockquote>
    <p>Foo</p>
    </blockquote>
    <hr />

</div>

</div>

<div id="example-63" class="example">

<div class="examplenum">

[Example 63](#example-63)

</div>

<div class="column">

    > foo
    bar
    ===

</div>

<div class="column">

    <blockquote>
    <p>foo
    bar
    ===</p>
    </blockquote>

</div>

</div>

<div id="example-64" class="example">

<div class="examplenum">

[Example 64](#example-64)

</div>

<div class="column">

    - Foo
    ---

</div>

<div class="column">

    <ul>
    <li>Foo</li>
    </ul>
    <hr />

</div>

</div>

A blank line is needed between a paragraph and a following setext
heading, since otherwise the paragraph becomes part of the heading’s
content:

<div id="example-65" class="example">

<div class="examplenum">

[Example 65](#example-65)

</div>

<div class="column">

    Foo
    Bar
    ---

</div>

<div class="column">

    <h2>Foo
    Bar</h2>

</div>

</div>

But in general a blank line is not required before or after setext
headings:

<div id="example-66" class="example">

<div class="examplenum">

[Example 66](#example-66)

</div>

<div class="column">

    ---
    Foo
    ---
    Bar
    ---
    Baz

</div>

<div class="column">

    <hr />
    <h2>Foo</h2>
    <h2>Bar</h2>
    <p>Baz</p>

</div>

</div>

Setext headings cannot be empty:

<div id="example-67" class="example">

<div class="examplenum">

[Example 67](#example-67)

</div>

<div class="column">

    ====

</div>

<div class="column">

    <p>====</p>

</div>

</div>

Setext heading text lines must not be interpretable as block constructs
other than paragraphs. So, the line of dashes in these examples gets
interpreted as a thematic break:

<div id="example-68" class="example">

<div class="examplenum">

[Example 68](#example-68)

</div>

<div class="column">

    ---
    ---

</div>

<div class="column">

    <hr />
    <hr />

</div>

</div>

<div id="example-69" class="example">

<div class="examplenum">

[Example 69](#example-69)

</div>

<div class="column">

    - foo
    -----

</div>

<div class="column">

    <ul>
    <li>foo</li>
    </ul>
    <hr />

</div>

</div>

<div id="example-70" class="example">

<div class="examplenum">

[Example 70](#example-70)

</div>

<div class="column">

``` 
    foo
---
```

</div>

<div class="column">

    <pre><code>foo
    </code></pre>
    <hr />

</div>

</div>

<div id="example-71" class="example">

<div class="examplenum">

[Example 71](#example-71)

</div>

<div class="column">

    > foo
    -----

</div>

<div class="column">

    <blockquote>
    <p>foo</p>
    </blockquote>
    <hr />

</div>

</div>

If you want a heading with `> foo` as its literal text, you can use
backslash escapes:

<div id="example-72" class="example">

<div class="examplenum">

[Example 72](#example-72)

</div>

<div class="column">

    \> foo
    ------

</div>

<div class="column">

    <h2>&gt; foo</h2>

</div>

</div>

**Compatibility note:** Most existing Markdown implementations do not
allow the text of setext headings to span multiple lines. But there is
no consensus about how to interpret

    Foo
    bar
    ---
    baz

One can find four different interpretations:

1.  paragraph “Foo”, heading “bar”, paragraph “baz”
2.  paragraph “Foo bar”, thematic break, paragraph “baz”
3.  paragraph “Foo bar — baz”
4.  heading “Foo bar”, paragraph “baz”

We find interpretation 4 most natural, and interpretation 4 increases
the expressive power of CommonMark, by allowing multiline headings.
Authors who want interpretation 1 can put a blank line after the first
paragraph:

<div id="example-73" class="example">

<div class="examplenum">

[Example 73](#example-73)

</div>

<div class="column">

    Foo
    
    bar
    ---
    baz

</div>

<div class="column">

    <p>Foo</p>
    <h2>bar</h2>
    <p>baz</p>

</div>

</div>

Authors who want interpretation 2 can put blank lines around the
thematic break,

<div id="example-74" class="example">

<div class="examplenum">

[Example 74](#example-74)

</div>

<div class="column">

    Foo
    bar
    
    ---
    
    baz

</div>

<div class="column">

    <p>Foo
    bar</p>
    <hr />
    <p>baz</p>

</div>

</div>

or use a thematic break that cannot count as a [setext heading
underline](#setext-heading-underline), such as

<div id="example-75" class="example">

<div class="examplenum">

[Example 75](#example-75)

</div>

<div class="column">

    Foo
    bar
    * * *
    baz

</div>

<div class="column">

    <p>Foo
    bar</p>
    <hr />
    <p>baz</p>

</div>

</div>

Authors who want interpretation 3 can use backslash escapes:

<div id="example-76" class="example">

<div class="examplenum">

[Example 76](#example-76)

</div>

<div class="column">

    Foo
    bar
    \---
    baz

</div>

<div class="column">

    <p>Foo
    bar
    ---
    baz</p>

</div>

</div>

## [](#TOC)<span class="number">4.4</span>Indented code blocks

An [indented code block](#indented-code-block) is composed of one or
more [indented chunks](#indented-chunk) separated by blank lines. An
[indented chunk](#indented-chunk) is a sequence of non-blank lines, each
indented four or more spaces. The contents of the code block are the
literal contents of the lines, including trailing [line
endings](#line-ending), minus four spaces of indentation. An indented
code block has no [info string](#info-string).

An indented code block cannot interrupt a paragraph, so there must be a
blank line between a paragraph and a following indented code block. (A
blank line is not needed, however, between a code block and a following
paragraph.)

<div id="example-77" class="example">

<div class="examplenum">

[Example 77](#example-77)

</div>

<div class="column">

``` 
    a simple
      indented code block
```

</div>

<div class="column">

    <pre><code>a simple
      indented code block
    </code></pre>

</div>

</div>

If there is any ambiguity between an interpretation of indentation as a
code block and as indicating that material belongs to a [list
item](#list-items), the list item interpretation takes precedence:

<div id="example-78" class="example">

<div class="examplenum">

[Example 78](#example-78)

</div>

<div class="column">

``` 
  - foo

    bar
```

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <p>bar</p>
    </li>
    </ul>

</div>

</div>

<div id="example-79" class="example">

<div class="examplenum">

[Example 79](#example-79)

</div>

<div class="column">

    1.  foo
    
        - bar

</div>

<div class="column">

    <ol>
    <li>
    <p>foo</p>
    <ul>
    <li>bar</li>
    </ul>
    </li>
    </ol>

</div>

</div>

The contents of a code block are literal text, and do not get parsed as
Markdown:

<div id="example-80" class="example">

<div class="examplenum">

[Example 80](#example-80)

</div>

<div class="column">

``` 
    <a/>
    *hi*

    - one
```

</div>

<div class="column">

    <pre><code>&lt;a/&gt;
    *hi*
    
    - one
    </code></pre>

</div>

</div>

Here we have three chunks separated by blank lines:

<div id="example-81" class="example">

<div class="examplenum">

[Example 81](#example-81)

</div>

<div class="column">

``` 
    chunk1

    chunk2
  
 
 
    chunk3
```

</div>

<div class="column">

    <pre><code>chunk1
    
    chunk2
    
    
    
    chunk3
    </code></pre>

</div>

</div>

Any initial spaces beyond four will be included in the content, even in
interior blank lines:

<div id="example-82" class="example">

<div class="examplenum">

[Example 82](#example-82)

</div>

<div class="column">

``` 
    chunk1
      
      chunk2
```

</div>

<div class="column">

    <pre><code>chunk1
      
      chunk2
    </code></pre>

</div>

</div>

An indented code block cannot interrupt a paragraph. (This allows
hanging indents and the like.)

<div id="example-83" class="example">

<div class="examplenum">

[Example 83](#example-83)

</div>

<div class="column">

    Foo
        bar

</div>

<div class="column">

    <p>Foo
    bar</p>

</div>

</div>

However, any non-blank line with fewer than four leading spaces ends the
code block immediately. So a paragraph may occur immediately after
indented code:

<div id="example-84" class="example">

<div class="examplenum">

[Example 84](#example-84)

</div>

<div class="column">

``` 
    foo
bar
```

</div>

<div class="column">

    <pre><code>foo
    </code></pre>
    <p>bar</p>

</div>

</div>

And indented code can occur immediately before and after other kinds of
blocks:

<div id="example-85" class="example">

<div class="examplenum">

[Example 85](#example-85)

</div>

<div class="column">

    # Heading
        foo
    Heading
    ------
        foo
    ----

</div>

<div class="column">

    <h1>Heading</h1>
    <pre><code>foo
    </code></pre>
    <h2>Heading</h2>
    <pre><code>foo
    </code></pre>
    <hr />

</div>

</div>

The first line can be indented more than four spaces:

<div id="example-86" class="example">

<div class="examplenum">

[Example 86](#example-86)

</div>

<div class="column">

``` 
        foo
    bar
```

</div>

<div class="column">

    <pre><code>    foo
    bar
    </code></pre>

</div>

</div>

Blank lines preceding or following an indented code block are not
included in it:

<div id="example-87" class="example">

<div class="examplenum">

[Example 87](#example-87)

</div>

<div class="column">

``` 
    
    foo
    
```

</div>

<div class="column">

    <pre><code>foo
    </code></pre>

</div>

</div>

Trailing spaces are included in the code block’s content:

<div id="example-88" class="example">

<div class="examplenum">

[Example 88](#example-88)

</div>

<div class="column">

``` 
    foo  
```

</div>

<div class="column">

    <pre><code>foo  
    </code></pre>

</div>

</div>

## [](#TOC)<span class="number">4.5</span>Fenced code blocks

A [code fence](#code-fence) is a sequence of at least three consecutive
backtick characters (`` ` ``) or tildes (`~`). (Tildes and backticks
cannot be mixed.) A [fenced code block](#fenced-code-block) begins with
a code fence, indented no more than three spaces.

The line with the opening code fence may optionally contain some text
following the code fence; this is trimmed of leading and trailing
whitespace and called the [info string](#info-string). If the [info
string](#info-string) comes after a backtick fence, it may not contain
any backtick characters. (The reason for this restriction is that
otherwise some inline code would be incorrectly interpreted as the
beginning of a fenced code block.)

The content of the code block consists of all subsequent lines, until a
closing [code fence](#code-fence) of the same type as the code block
began with (backticks or tildes), and with at least as many backticks or
tildes as the opening code fence. If the leading code fence is indented
N spaces, then up to N spaces of indentation are removed from each line
of the content (if present). (If a content line is not indented, it is
preserved unchanged. If it is indented less than N spaces, all of the
indentation is removed.)

The closing code fence may be indented up to three spaces, and may be
followed only by spaces, which are ignored. If the end of the containing
block (or document) is reached and no closing code fence has been found,
the code block contains all of the lines after the opening code fence
until the end of the containing block (or document). (An alternative
spec would require backtracking in the event that a closing code fence
is not found. But this makes parsing much less efficient, and there
seems to be no real down side to the behavior described here.)

A fenced code block may interrupt a paragraph, and does not require a
blank line either before or after.

The content of a code fence is treated as literal text, not parsed as
inlines. The first word of the [info string](#info-string) is typically
used to specify the language of the code sample, and rendered in the
`class` attribute of the `code` tag. However, this spec does not mandate
any particular treatment of the [info string](#info-string).

Here is a simple example with backticks:

<div id="example-89" class="example">

<div class="examplenum">

[Example 89](#example-89)

</div>

<div class="column">

    ```
    <
     >
    ```

</div>

<div class="column">

    <pre><code>&lt;
     &gt;
    </code></pre>

</div>

</div>

With tildes:

<div id="example-90" class="example">

<div class="examplenum">

[Example 90](#example-90)

</div>

<div class="column">

    ~~~
    <
     >
    ~~~

</div>

<div class="column">

    <pre><code>&lt;
     &gt;
    </code></pre>

</div>

</div>

Fewer than three backticks is not enough:

<div id="example-91" class="example">

<div class="examplenum">

[Example 91](#example-91)

</div>

<div class="column">

    ``
    foo
    ``

</div>

<div class="column">

    <p><code>foo</code></p>

</div>

</div>

The closing code fence must use the same character as the opening fence:

<div id="example-92" class="example">

<div class="examplenum">

[Example 92](#example-92)

</div>

<div class="column">

    ```
    aaa
    ~~~
    ```

</div>

<div class="column">

    <pre><code>aaa
    ~~~
    </code></pre>

</div>

</div>

<div id="example-93" class="example">

<div class="examplenum">

[Example 93](#example-93)

</div>

<div class="column">

    ~~~
    aaa
    ```
    ~~~

</div>

<div class="column">

    <pre><code>aaa
    ```
    </code></pre>

</div>

</div>

The closing code fence must be at least as long as the opening fence:

<div id="example-94" class="example">

<div class="examplenum">

[Example 94](#example-94)

</div>

<div class="column">

    ````
    aaa
    ```
    ``````

</div>

<div class="column">

    <pre><code>aaa
    ```
    </code></pre>

</div>

</div>

<div id="example-95" class="example">

<div class="examplenum">

[Example 95](#example-95)

</div>

<div class="column">

    ~~~~
    aaa
    ~~~
    ~~~~

</div>

<div class="column">

    <pre><code>aaa
    ~~~
    </code></pre>

</div>

</div>

Unclosed code blocks are closed by the end of the document (or the
enclosing [block quote](#block-quotes) or [list item](#list-items)):

<div id="example-96" class="example">

<div class="examplenum">

[Example 96](#example-96)

</div>

<div class="column">

    ```

</div>

<div class="column">

    <pre><code></code></pre>

</div>

</div>

<div id="example-97" class="example">

<div class="examplenum">

[Example 97](#example-97)

</div>

<div class="column">

    `````
    
    ```
    aaa

</div>

<div class="column">

    <pre><code>
    ```
    aaa
    </code></pre>

</div>

</div>

<div id="example-98" class="example">

<div class="examplenum">

[Example 98](#example-98)

</div>

<div class="column">

    > ```
    > aaa
    
    bbb

</div>

<div class="column">

    <blockquote>
    <pre><code>aaa
    </code></pre>
    </blockquote>
    <p>bbb</p>

</div>

</div>

A code block can have all empty lines as its content:

<div id="example-99" class="example">

<div class="examplenum">

[Example 99](#example-99)

</div>

<div class="column">

    ```
    
      
    ```

</div>

<div class="column">

    <pre><code>
      
    </code></pre>

</div>

</div>

A code block can be empty:

<div id="example-100" class="example">

<div class="examplenum">

[Example 100](#example-100)

</div>

<div class="column">

    ```
    ```

</div>

<div class="column">

    <pre><code></code></pre>

</div>

</div>

Fences can be indented. If the opening fence is indented, content lines
will have equivalent opening indentation removed, if present:

<div id="example-101" class="example">

<div class="examplenum">

[Example 101](#example-101)

</div>

<div class="column">

```` 
 ```
 aaa
aaa
```
````

</div>

<div class="column">

    <pre><code>aaa
    aaa
    </code></pre>

</div>

</div>

<div id="example-102" class="example">

<div class="examplenum">

[Example 102](#example-102)

</div>

<div class="column">

```` 
  ```
aaa
  aaa
aaa
  ```
````

</div>

<div class="column">

    <pre><code>aaa
    aaa
    aaa
    </code></pre>

</div>

</div>

<div id="example-103" class="example">

<div class="examplenum">

[Example 103](#example-103)

</div>

<div class="column">

```` 
   ```
   aaa
    aaa
  aaa
   ```
````

</div>

<div class="column">

    <pre><code>aaa
     aaa
    aaa
    </code></pre>

</div>

</div>

Four spaces indentation produces an indented code block:

<div id="example-104" class="example">

<div class="examplenum">

[Example 104](#example-104)

</div>

<div class="column">

```` 
    ```
    aaa
    ```
````

</div>

<div class="column">

    <pre><code>```
    aaa
    ```
    </code></pre>

</div>

</div>

Closing fences may be indented by 0-3 spaces, and their indentation need
not match that of the opening fence:

<div id="example-105" class="example">

<div class="examplenum">

[Example 105](#example-105)

</div>

<div class="column">

    ```
    aaa
      ```

</div>

<div class="column">

    <pre><code>aaa
    </code></pre>

</div>

</div>

<div id="example-106" class="example">

<div class="examplenum">

[Example 106](#example-106)

</div>

<div class="column">

```` 
   ```
aaa
  ```
````

</div>

<div class="column">

    <pre><code>aaa
    </code></pre>

</div>

</div>

This is not a closing fence, because it is indented 4 spaces:

<div id="example-107" class="example">

<div class="examplenum">

[Example 107](#example-107)

</div>

<div class="column">

    ```
    aaa
        ```

</div>

<div class="column">

    <pre><code>aaa
        ```
    </code></pre>

</div>

</div>

Code fences (opening and closing) cannot contain internal spaces:

<div id="example-108" class="example">

<div class="examplenum">

[Example 108](#example-108)

</div>

<div class="column">

    ``` ```
    aaa

</div>

<div class="column">

    <p><code> </code>
    aaa</p>

</div>

</div>

<div id="example-109" class="example">

<div class="examplenum">

[Example 109](#example-109)

</div>

<div class="column">

    ~~~~~~
    aaa
    ~~~ ~~

</div>

<div class="column">

    <pre><code>aaa
    ~~~ ~~
    </code></pre>

</div>

</div>

Fenced code blocks can interrupt paragraphs, and can be followed
directly by paragraphs, without a blank line between:

<div id="example-110" class="example">

<div class="examplenum">

[Example 110](#example-110)

</div>

<div class="column">

    foo
    ```
    bar
    ```
    baz

</div>

<div class="column">

    <p>foo</p>
    <pre><code>bar
    </code></pre>
    <p>baz</p>

</div>

</div>

Other blocks can also occur before and after fenced code blocks without
an intervening blank line:

<div id="example-111" class="example">

<div class="examplenum">

[Example 111](#example-111)

</div>

<div class="column">

    foo
    ---
    ~~~
    bar
    ~~~
    # baz

</div>

<div class="column">

    <h2>foo</h2>
    <pre><code>bar
    </code></pre>
    <h1>baz</h1>

</div>

</div>

An [info string](#info-string) can be provided after the opening code
fence. Although this spec doesn’t mandate any particular treatment of
the info string, the first word is typically used to specify the
language of the code block. In HTML output, the language is normally
indicated by adding a class to the `code` element consisting of
`language-` followed by the language name.

<div id="example-112" class="example">

<div class="examplenum">

[Example 112](#example-112)

</div>

<div class="column">

    ```ruby
    def foo(x)
      return 3
    end
    ```

</div>

<div class="column">

    <pre><code class="language-ruby">def foo(x)
      return 3
    end
    </code></pre>

</div>

</div>

<div id="example-113" class="example">

<div class="examplenum">

[Example 113](#example-113)

</div>

<div class="column">

    ~~~~    ruby startline=3 $%@#$
    def foo(x)
      return 3
    end
    ~~~~~~~

</div>

<div class="column">

    <pre><code class="language-ruby">def foo(x)
      return 3
    end
    </code></pre>

</div>

</div>

<div id="example-114" class="example">

<div class="examplenum">

[Example 114](#example-114)

</div>

<div class="column">

    ````;
    ````

</div>

<div class="column">

    <pre><code class="language-;"></code></pre>

</div>

</div>

[Info strings](#info-string) for backtick code blocks cannot contain
backticks:

<div id="example-115" class="example">

<div class="examplenum">

[Example 115](#example-115)

</div>

<div class="column">

    ``` aa ```
    foo

</div>

<div class="column">

    <p><code>aa</code>
    foo</p>

</div>

</div>

[Info strings](#info-string) for tilde code blocks can contain backticks
and tildes:

<div id="example-116" class="example">

<div class="examplenum">

[Example 116](#example-116)

</div>

<div class="column">

    ~~~ aa ``` ~~~
    foo
    ~~~

</div>

<div class="column">

    <pre><code class="language-aa">foo
    </code></pre>

</div>

</div>

Closing code fences cannot have [info strings](#info-string):

<div id="example-117" class="example">

<div class="examplenum">

[Example 117](#example-117)

</div>

<div class="column">

    ```
    ``` aaa
    ```

</div>

<div class="column">

    <pre><code>``` aaa
    </code></pre>

</div>

</div>

## [](#TOC)<span class="number">4.6</span>HTML blocks

An [HTML block](#html-block) is a group of lines that is treated as raw
HTML (and will not be escaped in HTML output).

There are seven kinds of [HTML block](#html-block), which can be defined
by their start and end conditions. The block begins with a line that
meets a [start condition](#start-condition) (after up to three spaces
optional indentation). It ends with the first subsequent line that meets
a matching [end condition](#end-condition), or the last line of the
document, or the last line of the [container block](#container-blocks)
containing the current HTML block, if no line is encountered that meets
the [end condition](#end-condition). If the first line meets both the
[start condition](#start-condition) and the [end
condition](#end-condition), the block will contain just that line.

1.  **Start condition:** line begins with the string `<script`, `<pre`,
    or `<style` (case-insensitive), followed by whitespace, the string
    `>`, or the end of the line.  
    **End condition:** line contains an end tag `</script>`, `</pre>`,
    or `</style>` (case-insensitive; it need not match the start tag).

2.  **Start condition:** line begins with the string `<!--`.  
    **End condition:** line contains the string `-->`.

3.  **Start condition:** line begins with the string `<?`.  
    **End condition:** line contains the string `?>`.

4.  **Start condition:** line begins with the string `<!` followed by an
    uppercase ASCII letter.  
    **End condition:** line contains the character `>`.

5.  **Start condition:** line begins with the string `<![CDATA[`.  
    **End condition:** line contains the string `]]>`.

6.  **Start condition:** line begins the string `<` or `</` followed by
    one of the strings (case-insensitive) `address`, `article`, `aside`,
    `base`, `basefont`, `blockquote`, `body`, `caption`, `center`,
    `col`, `colgroup`, `dd`, `details`, `dialog`, `dir`, `div`, `dl`,
    `dt`, `fieldset`, `figcaption`, `figure`, `footer`, `form`, `frame`,
    `frameset`, `h1`, `h2`, `h3`, `h4`, `h5`, `h6`, `head`, `header`,
    `hr`, `html`, `iframe`, `legend`, `li`, `link`, `main`, `menu`,
    `menuitem`, `nav`, `noframes`, `ol`, `optgroup`, `option`, `p`,
    `param`, `section`, `source`, `summary`, `table`, `tbody`, `td`,
    `tfoot`, `th`, `thead`, `title`, `tr`, `track`, `ul`, followed by
    [whitespace](#whitespace), the end of the line, the string `>`, or
    the string `/>`.  
    **End condition:** line is followed by a [blank line](#blank-line).

7.  **Start condition:** line begins with a complete [open
    tag](#open-tag) (with any [tag name](#tag-name) other than `script`,
    `style`, or `pre`) or a complete [closing tag](#closing-tag),
    followed only by [whitespace](#whitespace) or the end of the line.  
    **End condition:** line is followed by a [blank line](#blank-line).

HTML blocks continue until they are closed by their appropriate [end
condition](#end-condition), or the last line of the document or other
[container block](#container-blocks). This means any HTML **within an
HTML block** that might otherwise be recognised as a start condition
will be ignored by the parser and passed through as-is, without changing
the parser’s state.

For instance, `<pre>` within a HTML block started by `<table>` will not
affect the parser state; as the HTML block was started in by start
condition 6, it will end at any blank line. This can be surprising:

<div id="example-118" class="example">

<div class="examplenum">

[Example 118](#example-118)

</div>

<div class="column">

    <table><tr><td>
    <pre>
    **Hello**,
    
    _world_.
    </pre>
    </td></tr></table>

</div>

<div class="column">

    <table><tr><td>
    <pre>
    **Hello**,
    <p><em>world</em>.
    </pre></p>
    </td></tr></table>

</div>

</div>

In this case, the HTML block is terminated by the newline — the
`**Hello**` text remains verbatim — and regular parsing resumes, with a
paragraph, emphasised `world` and inline and block HTML following.

All types of [HTML blocks](#html-blocks) except type 7 may interrupt a
paragraph. Blocks of type 7 may not interrupt a paragraph. (This
restriction is intended to prevent unwanted interpretation of long tags
inside a wrapped paragraph as starting HTML blocks.)

Some simple examples follow. Here are some basic HTML blocks of type 6:

<div id="example-119" class="example">

<div class="examplenum">

[Example 119](#example-119)

</div>

<div class="column">

    <table>
      <tr>
        <td>
               hi
        </td>
      </tr>
    </table>
    
    okay.

</div>

<div class="column">

    <table>
      <tr>
        <td>
               hi
        </td>
      </tr>
    </table>
    <p>okay.</p>

</div>

</div>

<div id="example-120" class="example">

<div class="examplenum">

[Example 120](#example-120)

</div>

<div class="column">

``` 
 <div>
  *hello*
         <foo><a>
```

</div>

<div class="column">

``` 
 <div>
  *hello*
         <foo><a>
```

</div>

</div>

A block can also start with a closing tag:

<div id="example-121" class="example">

<div class="examplenum">

[Example 121](#example-121)

</div>

<div class="column">

    </div>
    *foo*

</div>

<div class="column">

    </div>
    *foo*

</div>

</div>

Here we have two HTML blocks with a Markdown paragraph between them:

<div id="example-122" class="example">

<div class="examplenum">

[Example 122](#example-122)

</div>

<div class="column">

    <DIV CLASS="foo">
    
    *Markdown*
    
    </DIV>

</div>

<div class="column">

    <DIV CLASS="foo">
    <p><em>Markdown</em></p>
    </DIV>

</div>

</div>

The tag on the first line can be partial, as long as it is split where
there would be whitespace:

<div id="example-123" class="example">

<div class="examplenum">

[Example 123](#example-123)

</div>

<div class="column">

    <div id="foo"
      class="bar">
    </div>

</div>

<div class="column">

    <div id="foo"
      class="bar">
    </div>

</div>

</div>

<div id="example-124" class="example">

<div class="examplenum">

[Example 124](#example-124)

</div>

<div class="column">

    <div id="foo" class="bar
      baz">
    </div>

</div>

<div class="column">

    <div id="foo" class="bar
      baz">
    </div>

</div>

</div>

An open tag need not be closed:

<div id="example-125" class="example">

<div class="examplenum">

[Example 125](#example-125)

</div>

<div class="column">

    <div>
    *foo*
    
    *bar*

</div>

<div class="column">

    <div>
    *foo*
    <p><em>bar</em></p>

</div>

</div>

A partial tag need not even be completed (garbage in, garbage out):

<div id="example-126" class="example">

<div class="examplenum">

[Example 126](#example-126)

</div>

<div class="column">

    <div id="foo"
    *hi*

</div>

<div class="column">

    <div id="foo"
    *hi*

</div>

</div>

<div id="example-127" class="example">

<div class="examplenum">

[Example 127](#example-127)

</div>

<div class="column">

    <div class
    foo

</div>

<div class="column">

    <div class
    foo

</div>

</div>

The initial tag doesn’t even need to be a valid tag, as long as it
starts like one:

<div id="example-128" class="example">

<div class="examplenum">

[Example 128](#example-128)

</div>

<div class="column">

    <div *???-&&&-<---
    *foo*

</div>

<div class="column">

    <div *???-&&&-<---
    *foo*

</div>

</div>

In type 6 blocks, the initial tag need not be on a line by itself:

<div id="example-129" class="example">

<div class="examplenum">

[Example 129](#example-129)

</div>

<div class="column">

    <div><a href="bar">*foo*</a></div>

</div>

<div class="column">

    <div><a href="bar">*foo*</a></div>

</div>

</div>

<div id="example-130" class="example">

<div class="examplenum">

[Example 130](#example-130)

</div>

<div class="column">

    <table><tr><td>
    foo
    </td></tr></table>

</div>

<div class="column">

    <table><tr><td>
    foo
    </td></tr></table>

</div>

</div>

Everything until the next blank line or end of document gets included in
the HTML block. So, in the following example, what looks like a Markdown
code block is actually part of the HTML block, which continues until a
blank line or the end of the document is reached:

<div id="example-131" class="example">

<div class="examplenum">

[Example 131](#example-131)

</div>

<div class="column">

    <div></div>
    ``` c
    int x = 33;
    ```

</div>

<div class="column">

    <div></div>
    ``` c
    int x = 33;
    ```

</div>

</div>

To start an [HTML block](#html-block) with a tag that is *not* in the
list of block-level tags in (6), you must put the tag by itself on the
first line (and it must be complete):

<div id="example-132" class="example">

<div class="examplenum">

[Example 132](#example-132)

</div>

<div class="column">

    <a href="foo">
    *bar*
    </a>

</div>

<div class="column">

    <a href="foo">
    *bar*
    </a>

</div>

</div>

In type 7 blocks, the [tag name](#tag-name) can be anything:

<div id="example-133" class="example">

<div class="examplenum">

[Example 133](#example-133)

</div>

<div class="column">

    <Warning>
    *bar*
    </Warning>

</div>

<div class="column">

    <Warning>
    *bar*
    </Warning>

</div>

</div>

<div id="example-134" class="example">

<div class="examplenum">

[Example 134](#example-134)

</div>

<div class="column">

    <i class="foo">
    *bar*
    </i>

</div>

<div class="column">

    <i class="foo">
    *bar*
    </i>

</div>

</div>

<div id="example-135" class="example">

<div class="examplenum">

[Example 135](#example-135)

</div>

<div class="column">

    </ins>
    *bar*

</div>

<div class="column">

    </ins>
    *bar*

</div>

</div>

These rules are designed to allow us to work with tags that can function
as either block-level or inline-level tags. The `<del>` tag is a nice
example. We can surround content with `<del>` tags in three different
ways. In this case, we get a raw HTML block, because the `<del>` tag is
on a line by itself:

<div id="example-136" class="example">

<div class="examplenum">

[Example 136](#example-136)

</div>

<div class="column">

    <del>
    *foo*
    </del>

</div>

<div class="column">

    <del>
    *foo*
    </del>

</div>

</div>

In this case, we get a raw HTML block that just includes the `<del>` tag
(because it ends with the following blank line). So the contents get
interpreted as CommonMark:

<div id="example-137" class="example">

<div class="examplenum">

[Example 137](#example-137)

</div>

<div class="column">

    <del>
    
    *foo*
    
    </del>

</div>

<div class="column">

    <del>
    <p><em>foo</em></p>
    </del>

</div>

</div>

Finally, in this case, the `<del>` tags are interpreted as [raw
HTML](#raw-html) *inside* the CommonMark paragraph. (Because the tag is
not on a line by itself, we get inline HTML rather than an [HTML
block](#html-block).)

<div id="example-138" class="example">

<div class="examplenum">

[Example 138](#example-138)

</div>

<div class="column">

    <del>*foo*</del>

</div>

<div class="column">

    <p><del><em>foo</em></del></p>

</div>

</div>

HTML tags designed to contain literal content (`script`, `style`,
`pre`), comments, processing instructions, and declarations are treated
somewhat differently. Instead of ending at the first blank line, these
blocks end at the first line containing a corresponding end tag. As a
result, these blocks can contain blank lines:

A pre tag (type 1):

<div id="example-139" class="example">

<div class="examplenum">

[Example 139](#example-139)

</div>

<div class="column">

    <pre language="haskell"><code>
    import Text.HTML.TagSoup
    
    main :: IO ()
    main = print $ parseTags tags
    </code></pre>
    okay

</div>

<div class="column">

    <pre language="haskell"><code>
    import Text.HTML.TagSoup
    
    main :: IO ()
    main = print $ parseTags tags
    </code></pre>
    <p>okay</p>

</div>

</div>

A script tag (type 1):

<div id="example-140" class="example">

<div class="examplenum">

[Example 140](#example-140)

</div>

<div class="column">

    <script type="text/javascript">
    // JavaScript example
    
    document.getElementById("demo").innerHTML = "Hello JavaScript!";
    </script>
    okay

</div>

<div class="column">

    <script type="text/javascript">
    // JavaScript example
    
    document.getElementById("demo").innerHTML = "Hello JavaScript!";
    </script>
    <p>okay</p>

</div>

</div>

A style tag (type 1):

<div id="example-141" class="example">

<div class="examplenum">

[Example 141](#example-141)

</div>

<div class="column">

    <style
      type="text/css">
    h1 {color:red;}
    
    p {color:blue;}
    </style>
    okay

</div>

<div class="column">

    <style
      type="text/css">
    h1 {color:red;}
    
    p {color:blue;}
    </style>
    <p>okay</p>

</div>

</div>

If there is no matching end tag, the block will end at the end of the
document (or the enclosing [block quote](#block-quotes) or [list
item](#list-items)):

<div id="example-142" class="example">

<div class="examplenum">

[Example 142](#example-142)

</div>

<div class="column">

    <style
      type="text/css">
    
    foo

</div>

<div class="column">

    <style
      type="text/css">
    
    foo

</div>

</div>

<div id="example-143" class="example">

<div class="examplenum">

[Example 143](#example-143)

</div>

<div class="column">

    > <div>
    > foo
    
    bar

</div>

<div class="column">

    <blockquote>
    <div>
    foo
    </blockquote>
    <p>bar</p>

</div>

</div>

<div id="example-144" class="example">

<div class="examplenum">

[Example 144](#example-144)

</div>

<div class="column">

    - <div>
    - foo

</div>

<div class="column">

    <ul>
    <li>
    <div>
    </li>
    <li>foo</li>
    </ul>

</div>

</div>

The end tag can occur on the same line as the start tag:

<div id="example-145" class="example">

<div class="examplenum">

[Example 145](#example-145)

</div>

<div class="column">

    <style>p{color:red;}</style>
    *foo*

</div>

<div class="column">

    <style>p{color:red;}</style>
    <p><em>foo</em></p>

</div>

</div>

<div id="example-146" class="example">

<div class="examplenum">

[Example 146](#example-146)

</div>

<div class="column">

    <!-- foo -->*bar*
    *baz*

</div>

<div class="column">

    <!-- foo -->*bar*
    <p><em>baz</em></p>

</div>

</div>

Note that anything on the last line after the end tag will be included
in the [HTML block](#html-block):

<div id="example-147" class="example">

<div class="examplenum">

[Example 147](#example-147)

</div>

<div class="column">

    <script>
    foo
    </script>1. *bar*

</div>

<div class="column">

    <script>
    foo
    </script>1. *bar*

</div>

</div>

A comment (type 2):

<div id="example-148" class="example">

<div class="examplenum">

[Example 148](#example-148)

</div>

<div class="column">

    <!-- Foo
    
    bar
       baz -->
    okay

</div>

<div class="column">

    <!-- Foo
    
    bar
       baz -->
    <p>okay</p>

</div>

</div>

A processing instruction (type 3):

<div id="example-149" class="example">

<div class="examplenum">

[Example 149](#example-149)

</div>

<div class="column">

    <?php
    
      echo '>';
    
    ?>
    okay

</div>

<div class="column">

    <?php
    
      echo '>';
    
    ?>
    <p>okay</p>

</div>

</div>

A declaration (type 4):

<div id="example-150" class="example">

<div class="examplenum">

[Example 150](#example-150)

</div>

<div class="column">

    <!DOCTYPE html>

</div>

<div class="column">

    <!DOCTYPE html>

</div>

</div>

CDATA (type 5):

<div id="example-151" class="example">

<div class="examplenum">

[Example 151](#example-151)

</div>

<div class="column">

    <![CDATA[
    function matchwo(a,b)
    {
      if (a < b && a < 0) then {
        return 1;
    
      } else {
    
        return 0;
      }
    }
    ]]>
    okay

</div>

<div class="column">

    <![CDATA[
    function matchwo(a,b)
    {
      if (a < b && a < 0) then {
        return 1;
    
      } else {
    
        return 0;
      }
    }
    ]]>
    <p>okay</p>

</div>

</div>

The opening tag can be indented 1-3 spaces, but not 4:

<div id="example-152" class="example">

<div class="examplenum">

[Example 152](#example-152)

</div>

<div class="column">

``` 
  <!-- foo -->

    <!-- foo -->
```

</div>

<div class="column">

``` 
  <!-- foo -->
<pre><code>&lt;!-- foo --&gt;
</code></pre>
```

</div>

</div>

<div id="example-153" class="example">

<div class="examplenum">

[Example 153](#example-153)

</div>

<div class="column">

``` 
  <div>

    <div>
```

</div>

<div class="column">

``` 
  <div>
<pre><code>&lt;div&gt;
</code></pre>
```

</div>

</div>

An HTML block of types 1–6 can interrupt a paragraph, and need not be
preceded by a blank line.

<div id="example-154" class="example">

<div class="examplenum">

[Example 154](#example-154)

</div>

<div class="column">

    Foo
    <div>
    bar
    </div>

</div>

<div class="column">

    <p>Foo</p>
    <div>
    bar
    </div>

</div>

</div>

However, a following blank line is needed, except at the end of a
document, and except for blocks of types 1–5, [above](#html-block):

<div id="example-155" class="example">

<div class="examplenum">

[Example 155](#example-155)

</div>

<div class="column">

    <div>
    bar
    </div>
    *foo*

</div>

<div class="column">

    <div>
    bar
    </div>
    *foo*

</div>

</div>

HTML blocks of type 7 cannot interrupt a paragraph:

<div id="example-156" class="example">

<div class="examplenum">

[Example 156](#example-156)

</div>

<div class="column">

    Foo
    <a href="bar">
    baz

</div>

<div class="column">

    <p>Foo
    <a href="bar">
    baz</p>

</div>

</div>

This rule differs from John Gruber’s original Markdown syntax
specification, which says:

> The only restrictions are that block-level HTML elements — e.g.
> `<div>`, `<table>`, `<pre>`, `<p>`, etc. — must be separated from
> surrounding content by blank lines, and the start and end tags of the
> block should not be indented with tabs or spaces.

In some ways Gruber’s rule is more restrictive than the one given here:

  - It requires that an HTML block be preceded by a blank line.
  - It does not allow the start tag to be indented.
  - It requires a matching end tag, which it also does not allow to be
    indented.

Most Markdown implementations (including some of Gruber’s own) do not
respect all of these restrictions.

There is one respect, however, in which Gruber’s rule is more liberal
than the one given here, since it allows blank lines to occur inside an
HTML block. There are two reasons for disallowing them here. First, it
removes the need to parse balanced tags, which is expensive and can
require backtracking from the end of the document if no matching end tag
is found. Second, it provides a very simple and flexible way of
including Markdown content inside HTML tags: simply separate the
Markdown from the HTML using blank lines:

Compare:

<div id="example-157" class="example">

<div class="examplenum">

[Example 157](#example-157)

</div>

<div class="column">

    <div>
    
    *Emphasized* text.
    
    </div>

</div>

<div class="column">

    <div>
    <p><em>Emphasized</em> text.</p>
    </div>

</div>

</div>

<div id="example-158" class="example">

<div class="examplenum">

[Example 158](#example-158)

</div>

<div class="column">

    <div>
    *Emphasized* text.
    </div>

</div>

<div class="column">

    <div>
    *Emphasized* text.
    </div>

</div>

</div>

Some Markdown implementations have adopted a convention of interpreting
content inside tags as text if the open tag has the attribute
`markdown=1`. The rule given above seems a simpler and more elegant way
of achieving the same expressive power, which is also much simpler to
parse.

The main potential drawback is that one can no longer paste HTML blocks
into Markdown documents with 100% reliability. However, *in most cases*
this will work fine, because the blank lines in HTML are usually
followed by HTML block tags. For example:

<div id="example-159" class="example">

<div class="examplenum">

[Example 159](#example-159)

</div>

<div class="column">

    <table>
    
    <tr>
    
    <td>
    Hi
    </td>
    
    </tr>
    
    </table>

</div>

<div class="column">

    <table>
    <tr>
    <td>
    Hi
    </td>
    </tr>
    </table>

</div>

</div>

There are problems, however, if the inner tags are indented *and*
separated by spaces, as then they will be interpreted as an indented
code block:

<div id="example-160" class="example">

<div class="examplenum">

[Example 160](#example-160)

</div>

<div class="column">

    <table>
    
      <tr>
    
        <td>
          Hi
        </td>
    
      </tr>
    
    </table>

</div>

<div class="column">

    <table>
      <tr>
    <pre><code>&lt;td&gt;
      Hi
    &lt;/td&gt;
    </code></pre>
      </tr>
    </table>

</div>

</div>

Fortunately, blank lines are usually not necessary and can be deleted.
The exception is inside `<pre>` tags, but as described
[above](#html-blocks), raw HTML blocks starting with `<pre>` *can*
contain blank lines.

## [](#TOC)<span class="number">4.7</span>Link reference definitions

A [link reference definition](#link-reference-definition) consists of a
[link label](#link-label), indented up to three spaces, followed by a
colon (`:`), optional [whitespace](#whitespace) (including up to one
[line ending](#line-ending)), a [link destination](#link-destination),
optional [whitespace](#whitespace) (including up to one [line
ending](#line-ending)), and an optional [link title](#link-title), which
if it is present must be separated from the [link
destination](#link-destination) by [whitespace](#whitespace). No further
[non-whitespace characters](#non-whitespace-character) may occur on the
line.

A [link reference definition](#link-reference-definition) does not
correspond to a structural element of a document. Instead, it defines a
label which can be used in [reference links](#reference-link) and
reference-style [images](#images) elsewhere in the document. [Link
reference definitions](#link-reference-definitions) can come either
before or after the links that use them.

<div id="example-161" class="example">

<div class="examplenum">

[Example 161](#example-161)

</div>

<div class="column">

    [foo]: /url "title"
    
    [foo]

</div>

<div class="column">

    <p><a href="/url" title="title">foo</a></p>

</div>

</div>

<div id="example-162" class="example">

<div class="examplenum">

[Example 162](#example-162)

</div>

<div class="column">

``` 
   [foo]: 
      /url  
           'the title'  

[foo]
```

</div>

<div class="column">

    <p><a href="/url" title="the title">foo</a></p>

</div>

</div>

<div id="example-163" class="example">

<div class="examplenum">

[Example 163](#example-163)

</div>

<div class="column">

    [Foo*bar\]]:my_(url) 'title (with parens)'
    
    [Foo*bar\]]

</div>

<div class="column">

    <p><a href="my_(url)" title="title (with parens)">Foo*bar]</a></p>

</div>

</div>

<div id="example-164" class="example">

<div class="examplenum">

[Example 164](#example-164)

</div>

<div class="column">

    [Foo bar]:
    <my url>
    'title'
    
    [Foo bar]

</div>

<div class="column">

    <p><a href="my%20url" title="title">Foo bar</a></p>

</div>

</div>

The title may extend over multiple lines:

<div id="example-165" class="example">

<div class="examplenum">

[Example 165](#example-165)

</div>

<div class="column">

    [foo]: /url '
    title
    line1
    line2
    '
    
    [foo]

</div>

<div class="column">

    <p><a href="/url" title="
    title
    line1
    line2
    ">foo</a></p>

</div>

</div>

However, it may not contain a [blank line](#blank-line):

<div id="example-166" class="example">

<div class="examplenum">

[Example 166](#example-166)

</div>

<div class="column">

    [foo]: /url 'title
    
    with blank line'
    
    [foo]

</div>

<div class="column">

    <p>[foo]: /url 'title</p>
    <p>with blank line'</p>
    <p>[foo]</p>

</div>

</div>

The title may be omitted:

<div id="example-167" class="example">

<div class="examplenum">

[Example 167](#example-167)

</div>

<div class="column">

    [foo]:
    /url
    
    [foo]

</div>

<div class="column">

    <p><a href="/url">foo</a></p>

</div>

</div>

The link destination may not be omitted:

<div id="example-168" class="example">

<div class="examplenum">

[Example 168](#example-168)

</div>

<div class="column">

    [foo]:
    
    [foo]

</div>

<div class="column">

    <p>[foo]:</p>
    <p>[foo]</p>

</div>

</div>

However, an empty link destination may be specified using angle
brackets:

<div id="example-169" class="example">

<div class="examplenum">

[Example 169](#example-169)

</div>

<div class="column">

    [foo]: <>
    
    [foo]

</div>

<div class="column">

    <p><a href="">foo</a></p>

</div>

</div>

The title must be separated from the link destination by whitespace:

<div id="example-170" class="example">

<div class="examplenum">

[Example 170](#example-170)

</div>

<div class="column">

    [foo]: <bar>(baz)
    
    [foo]

</div>

<div class="column">

    <p>[foo]: <bar>(baz)</p>
    <p>[foo]</p>

</div>

</div>

Both title and destination can contain backslash escapes and literal
backslashes:

<div id="example-171" class="example">

<div class="examplenum">

[Example 171](#example-171)

</div>

<div class="column">

    [foo]: /url\bar\*baz "foo\"bar\baz"
    
    [foo]

</div>

<div class="column">

    <p><a href="/url%5Cbar*baz" title="foo&quot;bar\baz">foo</a></p>

</div>

</div>

A link can come before its corresponding definition:

<div id="example-172" class="example">

<div class="examplenum">

[Example 172](#example-172)

</div>

<div class="column">

    [foo]
    
    [foo]: url

</div>

<div class="column">

    <p><a href="url">foo</a></p>

</div>

</div>

If there are several matching definitions, the first one takes
precedence:

<div id="example-173" class="example">

<div class="examplenum">

[Example 173](#example-173)

</div>

<div class="column">

    [foo]
    
    [foo]: first
    [foo]: second

</div>

<div class="column">

    <p><a href="first">foo</a></p>

</div>

</div>

As noted in the section on [Links](#links), matching of labels is
case-insensitive (see [matches](#matches)).

<div id="example-174" class="example">

<div class="examplenum">

[Example 174](#example-174)

</div>

<div class="column">

    [FOO]: /url
    
    [Foo]

</div>

<div class="column">

    <p><a href="/url">Foo</a></p>

</div>

</div>

<div id="example-175" class="example">

<div class="examplenum">

[Example 175](#example-175)

</div>

<div class="column">

    [ΑΓΩ]: /φου
    
    [αγω]

</div>

<div class="column">

    <p><a href="/%CF%86%CE%BF%CF%85">αγω</a></p>

</div>

</div>

Here is a link reference definition with no corresponding link. It
contributes nothing to the document.

<div id="example-176" class="example">

<div class="examplenum">

[Example 176](#example-176)

</div>

<div class="column">

    [foo]: /url

</div>

<div class="column">

``` 
```

</div>

</div>

Here is another one:

<div id="example-177" class="example">

<div class="examplenum">

[Example 177](#example-177)

</div>

<div class="column">

    [
    foo
    ]: /url
    bar

</div>

<div class="column">

    <p>bar</p>

</div>

</div>

This is not a link reference definition, because there are
[non-whitespace characters](#non-whitespace-character) after the title:

<div id="example-178" class="example">

<div class="examplenum">

[Example 178](#example-178)

</div>

<div class="column">

    [foo]: /url "title" ok

</div>

<div class="column">

    <p>[foo]: /url &quot;title&quot; ok</p>

</div>

</div>

This is a link reference definition, but it has no title:

<div id="example-179" class="example">

<div class="examplenum">

[Example 179](#example-179)

</div>

<div class="column">

    [foo]: /url
    "title" ok

</div>

<div class="column">

    <p>&quot;title&quot; ok</p>

</div>

</div>

This is not a link reference definition, because it is indented four
spaces:

<div id="example-180" class="example">

<div class="examplenum">

[Example 180](#example-180)

</div>

<div class="column">

``` 
    [foo]: /url "title"

[foo]
```

</div>

<div class="column">

    <pre><code>[foo]: /url &quot;title&quot;
    </code></pre>
    <p>[foo]</p>

</div>

</div>

This is not a link reference definition, because it occurs inside a code
block:

<div id="example-181" class="example">

<div class="examplenum">

[Example 181](#example-181)

</div>

<div class="column">

    ```
    [foo]: /url
    ```
    
    [foo]

</div>

<div class="column">

    <pre><code>[foo]: /url
    </code></pre>
    <p>[foo]</p>

</div>

</div>

A [link reference definition](#link-reference-definition) cannot
interrupt a paragraph.

<div id="example-182" class="example">

<div class="examplenum">

[Example 182](#example-182)

</div>

<div class="column">

    Foo
    [bar]: /baz
    
    [bar]

</div>

<div class="column">

    <p>Foo
    [bar]: /baz</p>
    <p>[bar]</p>

</div>

</div>

However, it can directly follow other block elements, such as headings
and thematic breaks, and it need not be followed by a blank line.

<div id="example-183" class="example">

<div class="examplenum">

[Example 183](#example-183)

</div>

<div class="column">

    # [Foo]
    [foo]: /url
    > bar

</div>

<div class="column">

    <h1><a href="/url">Foo</a></h1>
    <blockquote>
    <p>bar</p>
    </blockquote>

</div>

</div>

<div id="example-184" class="example">

<div class="examplenum">

[Example 184](#example-184)

</div>

<div class="column">

    [foo]: /url
    bar
    ===
    [foo]

</div>

<div class="column">

    <h1>bar</h1>
    <p><a href="/url">foo</a></p>

</div>

</div>

<div id="example-185" class="example">

<div class="examplenum">

[Example 185](#example-185)

</div>

<div class="column">

    [foo]: /url
    ===
    [foo]

</div>

<div class="column">

    <p>===
    <a href="/url">foo</a></p>

</div>

</div>

Several [link reference definitions](#link-reference-definitions) can
occur one after another, without intervening blank lines.

<div id="example-186" class="example">

<div class="examplenum">

[Example 186](#example-186)

</div>

<div class="column">

    [foo]: /foo-url "foo"
    [bar]: /bar-url
      "bar"
    [baz]: /baz-url
    
    [foo],
    [bar],
    [baz]

</div>

<div class="column">

    <p><a href="/foo-url" title="foo">foo</a>,
    <a href="/bar-url" title="bar">bar</a>,
    <a href="/baz-url">baz</a></p>

</div>

</div>

[Link reference definitions](#link-reference-definitions) can occur
inside block containers, like lists and block quotations. They affect
the entire document, not just the container in which they are defined:

<div id="example-187" class="example">

<div class="examplenum">

[Example 187](#example-187)

</div>

<div class="column">

    [foo]
    
    > [foo]: /url

</div>

<div class="column">

    <p><a href="/url">foo</a></p>
    <blockquote>
    </blockquote>

</div>

</div>

Whether something is a [link reference
definition](#link-reference-definition) is independent of whether the
link reference it defines is used in the document. Thus, for example,
the following document contains just a link reference definition, and no
visible content:

<div id="example-188" class="example">

<div class="examplenum">

[Example 188](#example-188)

</div>

<div class="column">

    [foo]: /url

</div>

<div class="column">

``` 
```

</div>

</div>

## [](#TOC)<span class="number">4.8</span>Paragraphs

A sequence of non-blank lines that cannot be interpreted as other kinds
of blocks forms a [paragraph](#paragraph). The contents of the paragraph
are the result of parsing the paragraph’s raw content as inlines. The
paragraph’s raw content is formed by concatenating the lines and
removing initial and final [whitespace](#whitespace).

A simple example with two paragraphs:

<div id="example-189" class="example">

<div class="examplenum">

[Example 189](#example-189)

</div>

<div class="column">

    aaa
    
    bbb

</div>

<div class="column">

    <p>aaa</p>
    <p>bbb</p>

</div>

</div>

Paragraphs can contain multiple lines, but no blank lines:

<div id="example-190" class="example">

<div class="examplenum">

[Example 190](#example-190)

</div>

<div class="column">

    aaa
    bbb
    
    ccc
    ddd

</div>

<div class="column">

    <p>aaa
    bbb</p>
    <p>ccc
    ddd</p>

</div>

</div>

Multiple blank lines between paragraph have no effect:

<div id="example-191" class="example">

<div class="examplenum">

[Example 191](#example-191)

</div>

<div class="column">

    aaa
    
    
    bbb

</div>

<div class="column">

    <p>aaa</p>
    <p>bbb</p>

</div>

</div>

Leading spaces are skipped:

<div id="example-192" class="example">

<div class="examplenum">

[Example 192](#example-192)

</div>

<div class="column">

``` 
  aaa
 bbb
```

</div>

<div class="column">

    <p>aaa
    bbb</p>

</div>

</div>

Lines after the first may be indented any amount, since indented code
blocks cannot interrupt paragraphs.

<div id="example-193" class="example">

<div class="examplenum">

[Example 193](#example-193)

</div>

<div class="column">

    aaa
                 bbb
                                           ccc

</div>

<div class="column">

    <p>aaa
    bbb
    ccc</p>

</div>

</div>

However, the first line may be indented at most three spaces, or an
indented code block will be triggered:

<div id="example-194" class="example">

<div class="examplenum">

[Example 194](#example-194)

</div>

<div class="column">

``` 
   aaa
bbb
```

</div>

<div class="column">

    <p>aaa
    bbb</p>

</div>

</div>

<div id="example-195" class="example">

<div class="examplenum">

[Example 195](#example-195)

</div>

<div class="column">

``` 
    aaa
bbb
```

</div>

<div class="column">

    <pre><code>aaa
    </code></pre>
    <p>bbb</p>

</div>

</div>

Final spaces are stripped before inline parsing, so a paragraph that
ends with two or more spaces will not end with a [hard line
break](#hard-line-break):

<div id="example-196" class="example">

<div class="examplenum">

[Example 196](#example-196)

</div>

<div class="column">

``` 
aaa     
bbb     
```

</div>

<div class="column">

    <p>aaa<br />
    bbb</p>

</div>

</div>

## [](#TOC)<span class="number">4.9</span>Blank lines

[Blank lines](#blank-lines) between block-level elements are ignored,
except for the role they play in determining whether a [list](#list) is
[tight](#tight) or [loose](#loose).

Blank lines at the beginning and end of the document are also ignored.

<div id="example-197" class="example">

<div class="examplenum">

[Example 197](#example-197)

</div>

<div class="column">

``` 
  

aaa
  

# aaa

  
```

</div>

<div class="column">

    <p>aaa</p>
    <h1>aaa</h1>

</div>

</div>

<div class="extension">

## [](#TOC)<span class="number">4.10</span>Tables (extension)

GFM enables the `table` extension, where an additional leaf block type
is available.

A [table](#table) is an arrangement of data with rows and columns,
consisting of a single header row, a [delimiter row](#delimiter-row)
separating the header from the data, and zero or more data rows.

Each row consists of cells containing arbitrary text, in which
[inlines](#inline) are parsed, separated by pipes (`|`). A leading and
trailing pipe is also recommended for clarity of reading, and if there’s
otherwise parsing ambiguity. Spaces between pipes and cell content are
trimmed. Block-level elements cannot be inserted in a table.

The [delimiter row](#delimiter-row) consists of cells whose only content
are hyphens (`-`), and optionally, a leading or trailing colon (`:`), or
both, to indicate left, right, or center alignment respectively.

<div id="example-198" class="example">

<div class="examplenum">

[Example 198](#example-198)

</div>

<div class="column">

    | foo | bar |
    | --- | --- |
    | baz | bim |

</div>

<div class="column">

    <table>
    <thead>
    <tr>
    <th>foo</th>
    <th>bar</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>baz</td>
    <td>bim</td>
    </tr>
    </tbody>
    </table>

</div>

</div>

Cells in one column don’t need to match length, though it’s easier to
read if they are. Likewise, use of leading and trailing pipes may be
inconsistent:

<div id="example-199" class="example">

<div class="examplenum">

[Example 199](#example-199)

</div>

<div class="column">

    | abc | defghi |
    :-: | -----------:
    bar | baz

</div>

<div class="column">

    <table>
    <thead>
    <tr>
    <th align="center">abc</th>
    <th align="right">defghi</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="center">bar</td>
    <td align="right">baz</td>
    </tr>
    </tbody>
    </table>

</div>

</div>

Include a pipe in a cell’s content by escaping it, including inside
other inline spans:

<div id="example-200" class="example">

<div class="examplenum">

[Example 200](#example-200)

</div>

<div class="column">

    | f\|oo  |
    | ------ |
    | b `\|` az |
    | b **\|** im |

</div>

<div class="column">

    <table>
    <thead>
    <tr>
    <th>f|oo</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>b <code>|</code> az</td>
    </tr>
    <tr>
    <td>b <strong>|</strong> im</td>
    </tr>
    </tbody>
    </table>

</div>

</div>

The table is broken at the first empty line, or beginning of another
block-level structure:

<div id="example-201" class="example">

<div class="examplenum">

[Example 201](#example-201)

</div>

<div class="column">

    | abc | def |
    | --- | --- |
    | bar | baz |
    > bar

</div>

<div class="column">

    <table>
    <thead>
    <tr>
    <th>abc</th>
    <th>def</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>bar</td>
    <td>baz</td>
    </tr>
    </tbody>
    </table>
    <blockquote>
    <p>bar</p>
    </blockquote>

</div>

</div>

<div id="example-202" class="example">

<div class="examplenum">

[Example 202](#example-202)

</div>

<div class="column">

    | abc | def |
    | --- | --- |
    | bar | baz |
    bar
    
    bar

</div>

<div class="column">

    <table>
    <thead>
    <tr>
    <th>abc</th>
    <th>def</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>bar</td>
    <td>baz</td>
    </tr>
    <tr>
    <td>bar</td>
    <td></td>
    </tr>
    </tbody>
    </table>
    <p>bar</p>

</div>

</div>

The header row must match the [delimiter row](#delimiter-row) in the
number of cells. If not, a table will not be recognized:

<div id="example-203" class="example">

<div class="examplenum">

[Example 203](#example-203)

</div>

<div class="column">

    | abc | def |
    | --- |
    | bar |

</div>

<div class="column">

    <p>| abc | def |
    | --- |
    | bar |</p>

</div>

</div>

The remainder of the table’s rows may vary in the number of cells. If
there are a number of cells fewer than the number of cells in the header
row, empty cells are inserted. If there are greater, the excess is
ignored:

<div id="example-204" class="example">

<div class="examplenum">

[Example 204](#example-204)

</div>

<div class="column">

    | abc | def |
    | --- | --- |
    | bar |
    | bar | baz | boo |

</div>

<div class="column">

    <table>
    <thead>
    <tr>
    <th>abc</th>
    <th>def</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>bar</td>
    <td></td>
    </tr>
    <tr>
    <td>bar</td>
    <td>baz</td>
    </tr>
    </tbody>
    </table>

</div>

</div>

If there are no rows in the body, no `<tbody>` is generated in HTML
output:

<div id="example-205" class="example">

<div class="examplenum">

[Example 205](#example-205)

</div>

<div class="column">

    | abc | def |
    | --- | --- |

</div>

<div class="column">

    <table>
    <thead>
    <tr>
    <th>abc</th>
    <th>def</th>
    </tr>
    </thead>
    </table>

</div>

</div>

</div>

# <span class="number">5</span>Container blocks

A [container block](#container-blocks) is a block that has other blocks
as its contents. There are two basic kinds of container blocks: [block
quotes](#block-quotes) and [list items](#list-items). [Lists](#lists)
are meta-containers for [list items](#list-items).

We define the syntax for container blocks recursively. The general form
of the definition is:

> If X is a sequence of blocks, then the result of transforming X in
> such-and-such a way is a container of type Y with these blocks as its
> content.

So, we explain what counts as a block quote or list item by explaining
how these can be *generated* from their contents. This should suffice to
define the syntax, although it does not give a recipe for *parsing*
these constructions. (A recipe is provided below in the section entitled
[A parsing strategy](#appendix-a-parsing-strategy).)

## [](#TOC)<span class="number">5.1</span>Block quotes

A [block quote marker](#block-quote-marker) consists of 0-3 spaces of
initial indent, plus (a) the character `>` together with a following
space, or (b) a single character `>` not followed by a space.

The following rules define [block quotes](#block-quotes):

1.  **Basic case.** If a string of lines *Ls* constitute a sequence of
    blocks *Bs*, then the result of prepending a [block quote
    marker](#block-quote-marker) to the beginning of each line in *Ls*
    is a [block quote](#block-quotes) containing *Bs*.

2.  **Laziness.** If a string of lines *Ls* constitute a [block
    quote](#block-quotes) with contents *Bs*, then the result of
    deleting the initial [block quote marker](#block-quote-marker) from
    one or more lines in which the next [non-whitespace
    character](#non-whitespace-character) after the [block quote
    marker](#block-quote-marker) is [paragraph continuation
    text](#paragraph-continuation-text) is a block quote with *Bs* as
    its content. [Paragraph continuation
    text](#paragraph-continuation-text) is text that will be parsed as
    part of the content of a paragraph, but does not occur at the
    beginning of the paragraph.

3.  **Consecutiveness.** A document cannot contain two [block
    quotes](#block-quotes) in a row unless there is a [blank
    line](#blank-line) between them.

Nothing else counts as a [block quote](#block-quotes).

Here is a simple example:

<div id="example-206" class="example">

<div class="examplenum">

[Example 206](#example-206)

</div>

<div class="column">

    > # Foo
    > bar
    > baz

</div>

<div class="column">

    <blockquote>
    <h1>Foo</h1>
    <p>bar
    baz</p>
    </blockquote>

</div>

</div>

The spaces after the `>` characters can be omitted:

<div id="example-207" class="example">

<div class="examplenum">

[Example 207](#example-207)

</div>

<div class="column">

    ># Foo
    >bar
    > baz

</div>

<div class="column">

    <blockquote>
    <h1>Foo</h1>
    <p>bar
    baz</p>
    </blockquote>

</div>

</div>

The `>` characters can be indented 1-3 spaces:

<div id="example-208" class="example">

<div class="examplenum">

[Example 208](#example-208)

</div>

<div class="column">

``` 
   > # Foo
   > bar
 > baz
```

</div>

<div class="column">

    <blockquote>
    <h1>Foo</h1>
    <p>bar
    baz</p>
    </blockquote>

</div>

</div>

Four spaces gives us a code block:

<div id="example-209" class="example">

<div class="examplenum">

[Example 209](#example-209)

</div>

<div class="column">

``` 
    > # Foo
    > bar
    > baz
```

</div>

<div class="column">

    <pre><code>&gt; # Foo
    &gt; bar
    &gt; baz
    </code></pre>

</div>

</div>

The Laziness clause allows us to omit the `>` before [paragraph
continuation text](#paragraph-continuation-text):

<div id="example-210" class="example">

<div class="examplenum">

[Example 210](#example-210)

</div>

<div class="column">

    > # Foo
    > bar
    baz

</div>

<div class="column">

    <blockquote>
    <h1>Foo</h1>
    <p>bar
    baz</p>
    </blockquote>

</div>

</div>

A block quote can contain some lazy and some non-lazy continuation
lines:

<div id="example-211" class="example">

<div class="examplenum">

[Example 211](#example-211)

</div>

<div class="column">

    > bar
    baz
    > foo

</div>

<div class="column">

    <blockquote>
    <p>bar
    baz
    foo</p>
    </blockquote>

</div>

</div>

Laziness only applies to lines that would have been continuations of
paragraphs had they been prepended with [block quote
markers](#block-quote-marker). For example, the `>` cannot be omitted in
the second line of

    > foo
    > ---

without changing the meaning:

<div id="example-212" class="example">

<div class="examplenum">

[Example 212](#example-212)

</div>

<div class="column">

    > foo
    ---

</div>

<div class="column">

    <blockquote>
    <p>foo</p>
    </blockquote>
    <hr />

</div>

</div>

Similarly, if we omit the `>` in the second line of

    > - foo
    > - bar

then the block quote ends after the first line:

<div id="example-213" class="example">

<div class="examplenum">

[Example 213](#example-213)

</div>

<div class="column">

    > - foo
    - bar

</div>

<div class="column">

    <blockquote>
    <ul>
    <li>foo</li>
    </ul>
    </blockquote>
    <ul>
    <li>bar</li>
    </ul>

</div>

</div>

For the same reason, we can’t omit the `>` in front of subsequent lines
of an indented or fenced code block:

<div id="example-214" class="example">

<div class="examplenum">

[Example 214](#example-214)

</div>

<div class="column">

    >     foo
        bar

</div>

<div class="column">

    <blockquote>
    <pre><code>foo
    </code></pre>
    </blockquote>
    <pre><code>bar
    </code></pre>

</div>

</div>

<div id="example-215" class="example">

<div class="examplenum">

[Example 215](#example-215)

</div>

<div class="column">

    > ```
    foo
    ```

</div>

<div class="column">

    <blockquote>
    <pre><code></code></pre>
    </blockquote>
    <p>foo</p>
    <pre><code></code></pre>

</div>

</div>

Note that in the following case, we have a [lazy continuation
line](#lazy-continuation-line):

<div id="example-216" class="example">

<div class="examplenum">

[Example 216](#example-216)

</div>

<div class="column">

    > foo
        - bar

</div>

<div class="column">

    <blockquote>
    <p>foo
    - bar</p>
    </blockquote>

</div>

</div>

To see why, note that in

    > foo
    >     - bar

the `- bar` is indented too far to start a list, and can’t be an
indented code block because indented code blocks cannot interrupt
paragraphs, so it is [paragraph continuation
text](#paragraph-continuation-text).

A block quote can be empty:

<div id="example-217" class="example">

<div class="examplenum">

[Example 217](#example-217)

</div>

<div class="column">

``` 
>
```

</div>

<div class="column">

    <blockquote>
    </blockquote>

</div>

</div>

<div id="example-218" class="example">

<div class="examplenum">

[Example 218](#example-218)

</div>

<div class="column">

    >
    >  
    > 

</div>

<div class="column">

    <blockquote>
    </blockquote>

</div>

</div>

A block quote can have initial or final blank lines:

<div id="example-219" class="example">

<div class="examplenum">

[Example 219](#example-219)

</div>

<div class="column">

``` 
>
> foo
>  
```

</div>

<div class="column">

    <blockquote>
    <p>foo</p>
    </blockquote>

</div>

</div>

A blank line always separates block quotes:

<div id="example-220" class="example">

<div class="examplenum">

[Example 220](#example-220)

</div>

<div class="column">

    > foo
    
    > bar

</div>

<div class="column">

    <blockquote>
    <p>foo</p>
    </blockquote>
    <blockquote>
    <p>bar</p>
    </blockquote>

</div>

</div>

(Most current Markdown implementations, including John Gruber’s original
`Markdown.pl`, will parse this example as a single block quote with two
paragraphs. But it seems better to allow the author to decide whether
two block quotes or one are wanted.)

Consecutiveness means that if we put these block quotes together, we get
a single block quote:

<div id="example-221" class="example">

<div class="examplenum">

[Example 221](#example-221)

</div>

<div class="column">

    > foo
    > bar

</div>

<div class="column">

    <blockquote>
    <p>foo
    bar</p>
    </blockquote>

</div>

</div>

To get a block quote with two paragraphs, use:

<div id="example-222" class="example">

<div class="examplenum">

[Example 222](#example-222)

</div>

<div class="column">

    > foo
    >
    > bar

</div>

<div class="column">

    <blockquote>
    <p>foo</p>
    <p>bar</p>
    </blockquote>

</div>

</div>

Block quotes can interrupt paragraphs:

<div id="example-223" class="example">

<div class="examplenum">

[Example 223](#example-223)

</div>

<div class="column">

    foo
    > bar

</div>

<div class="column">

    <p>foo</p>
    <blockquote>
    <p>bar</p>
    </blockquote>

</div>

</div>

In general, blank lines are not needed before or after block quotes:

<div id="example-224" class="example">

<div class="examplenum">

[Example 224](#example-224)

</div>

<div class="column">

    > aaa
    ***
    > bbb

</div>

<div class="column">

    <blockquote>
    <p>aaa</p>
    </blockquote>
    <hr />
    <blockquote>
    <p>bbb</p>
    </blockquote>

</div>

</div>

However, because of laziness, a blank line is needed between a block
quote and a following paragraph:

<div id="example-225" class="example">

<div class="examplenum">

[Example 225](#example-225)

</div>

<div class="column">

    > bar
    baz

</div>

<div class="column">

    <blockquote>
    <p>bar
    baz</p>
    </blockquote>

</div>

</div>

<div id="example-226" class="example">

<div class="examplenum">

[Example 226](#example-226)

</div>

<div class="column">

    > bar
    
    baz

</div>

<div class="column">

    <blockquote>
    <p>bar</p>
    </blockquote>
    <p>baz</p>

</div>

</div>

<div id="example-227" class="example">

<div class="examplenum">

[Example 227](#example-227)

</div>

<div class="column">

    > bar
    >
    baz

</div>

<div class="column">

    <blockquote>
    <p>bar</p>
    </blockquote>
    <p>baz</p>

</div>

</div>

It is a consequence of the Laziness rule that any number of initial `>`s
may be omitted on a continuation line of a nested block quote:

<div id="example-228" class="example">

<div class="examplenum">

[Example 228](#example-228)

</div>

<div class="column">

    > > > foo
    bar

</div>

<div class="column">

    <blockquote>
    <blockquote>
    <blockquote>
    <p>foo
    bar</p>
    </blockquote>
    </blockquote>
    </blockquote>

</div>

</div>

<div id="example-229" class="example">

<div class="examplenum">

[Example 229](#example-229)

</div>

<div class="column">

    >>> foo
    > bar
    >>baz

</div>

<div class="column">

    <blockquote>
    <blockquote>
    <blockquote>
    <p>foo
    bar
    baz</p>
    </blockquote>
    </blockquote>
    </blockquote>

</div>

</div>

When including an indented code block in a block quote, remember that
the [block quote marker](#block-quote-marker) includes both the `>` and
a following space. So *five spaces* are needed after the `>`:

<div id="example-230" class="example">

<div class="examplenum">

[Example 230](#example-230)

</div>

<div class="column">

    >     code
    
    >    not code

</div>

<div class="column">

    <blockquote>
    <pre><code>code
    </code></pre>
    </blockquote>
    <blockquote>
    <p>not code</p>
    </blockquote>

</div>

</div>

## [](#TOC)<span class="number">5.2</span>List items

A [list marker](#list-marker) is a [bullet list
marker](#bullet-list-marker) or an [ordered list
marker](#ordered-list-marker).

A [bullet list marker](#bullet-list-marker) is a `-`, `+`, or `*`
character.

An [ordered list marker](#ordered-list-marker) is a sequence of 1–9
arabic digits (`0-9`), followed by either a `.` character or a `)`
character. (The reason for the length limit is that with 10 digits we
start seeing integer overflows in some browsers.)

The following rules define [list items](#list-items):

1.  **Basic case.** If a sequence of lines *Ls* constitute a sequence of
    blocks *Bs* starting with a [non-whitespace
    character](#non-whitespace-character), and *M* is a list marker of
    width *W* followed by 1 ≤ *N* ≤ 4 spaces, then the result of
    prepending *M* and the following spaces to the first line of *Ls*,
    and indenting subsequent lines of *Ls* by *W + N* spaces, is a list
    item with *Bs* as its contents. The type of the list item (bullet or
    ordered) is determined by the type of its list marker. If the list
    item is ordered, then it is also assigned a start number, based on
    the ordered list marker.
    
    Exceptions:
    
    1.  When the first list item in a [list](#list) interrupts a
        paragraph—that is, when it starts on a line that would otherwise
        count as [paragraph continuation
        text](#paragraph-continuation-text)—then (a) the lines *Ls* must
        not begin with a blank line, and (b) if the list item is
        ordered, the start number must be 1.
    2.  If any line is a [thematic break](#thematic-breaks) then that
        line is not a list item.

For example, let *Ls* be the lines

<div id="example-231" class="example">

<div class="examplenum">

[Example 231](#example-231)

</div>

<div class="column">

    A paragraph
    with two lines.
    
        indented code
    
    > A block quote.

</div>

<div class="column">

    <p>A paragraph
    with two lines.</p>
    <pre><code>indented code
    </code></pre>
    <blockquote>
    <p>A block quote.</p>
    </blockquote>

</div>

</div>

And let *M* be the marker `1.`, and *N* = 2. Then rule \#1 says that the
following is an ordered list item with start number 1, and the same
contents as *Ls*:

<div id="example-232" class="example">

<div class="examplenum">

[Example 232](#example-232)

</div>

<div class="column">

    1.  A paragraph
        with two lines.
    
            indented code
    
        > A block quote.

</div>

<div class="column">

    <ol>
    <li>
    <p>A paragraph
    with two lines.</p>
    <pre><code>indented code
    </code></pre>
    <blockquote>
    <p>A block quote.</p>
    </blockquote>
    </li>
    </ol>

</div>

</div>

The most important thing to notice is that the position of the text
after the list marker determines how much indentation is needed in
subsequent blocks in the list item. If the list marker takes up two
spaces, and there are three spaces between the list marker and the next
[non-whitespace character](#non-whitespace-character), then blocks must
be indented five spaces in order to fall under the list item.

Here are some examples showing how far content must be indented to be
put under the list item:

<div id="example-233" class="example">

<div class="examplenum">

[Example 233](#example-233)

</div>

<div class="column">

    - one
    
     two

</div>

<div class="column">

    <ul>
    <li>one</li>
    </ul>
    <p>two</p>

</div>

</div>

<div id="example-234" class="example">

<div class="examplenum">

[Example 234](#example-234)

</div>

<div class="column">

    - one
    
      two

</div>

<div class="column">

    <ul>
    <li>
    <p>one</p>
    <p>two</p>
    </li>
    </ul>

</div>

</div>

<div id="example-235" class="example">

<div class="examplenum">

[Example 235](#example-235)

</div>

<div class="column">

``` 
 -    one

     two
```

</div>

<div class="column">

    <ul>
    <li>one</li>
    </ul>
    <pre><code> two
    </code></pre>

</div>

</div>

<div id="example-236" class="example">

<div class="examplenum">

[Example 236](#example-236)

</div>

<div class="column">

``` 
 -    one

      two
```

</div>

<div class="column">

    <ul>
    <li>
    <p>one</p>
    <p>two</p>
    </li>
    </ul>

</div>

</div>

It is tempting to think of this in terms of columns: the continuation
blocks must be indented at least to the column of the first
[non-whitespace character](#non-whitespace-character) after the list
marker. However, that is not quite right. The spaces after the list
marker determine how much relative indentation is needed. Which column
this indentation reaches will depend on how the list item is embedded in
other constructions, as shown by this example:

<div id="example-237" class="example">

<div class="examplenum">

[Example 237](#example-237)

</div>

<div class="column">

``` 
   > > 1.  one
>>
>>     two
```

</div>

<div class="column">

    <blockquote>
    <blockquote>
    <ol>
    <li>
    <p>one</p>
    <p>two</p>
    </li>
    </ol>
    </blockquote>
    </blockquote>

</div>

</div>

Here `two` occurs in the same column as the list marker `1.`, but is
actually contained in the list item, because there is sufficient
indentation after the last containing blockquote marker.

The converse is also possible. In the following example, the word `two`
occurs far to the right of the initial text of the list item, `one`, but
it is not considered part of the list item, because it is not indented
far enough past the blockquote marker:

<div id="example-238" class="example">

<div class="examplenum">

[Example 238](#example-238)

</div>

<div class="column">

    >>- one
    >>
      >  > two

</div>

<div class="column">

    <blockquote>
    <blockquote>
    <ul>
    <li>one</li>
    </ul>
    <p>two</p>
    </blockquote>
    </blockquote>

</div>

</div>

Note that at least one space is needed between the list marker and any
following content, so these are not list items:

<div id="example-239" class="example">

<div class="examplenum">

[Example 239](#example-239)

</div>

<div class="column">

    -one
    
    2.two

</div>

<div class="column">

    <p>-one</p>
    <p>2.two</p>

</div>

</div>

A list item may contain blocks that are separated by more than one blank
line.

<div id="example-240" class="example">

<div class="examplenum">

[Example 240](#example-240)

</div>

<div class="column">

    - foo
    
    
      bar

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <p>bar</p>
    </li>
    </ul>

</div>

</div>

A list item may contain any kind of block:

<div id="example-241" class="example">

<div class="examplenum">

[Example 241](#example-241)

</div>

<div class="column">

    1.  foo
    
        ```
        bar
        ```
    
        baz
    
        > bam

</div>

<div class="column">

    <ol>
    <li>
    <p>foo</p>
    <pre><code>bar
    </code></pre>
    <p>baz</p>
    <blockquote>
    <p>bam</p>
    </blockquote>
    </li>
    </ol>

</div>

</div>

A list item that contains an indented code block will preserve empty
lines within the code block verbatim.

<div id="example-242" class="example">

<div class="examplenum">

[Example 242](#example-242)

</div>

<div class="column">

    - Foo
    
          bar
    
    
          baz

</div>

<div class="column">

    <ul>
    <li>
    <p>Foo</p>
    <pre><code>bar
    
    
    baz
    </code></pre>
    </li>
    </ul>

</div>

</div>

Note that ordered list start numbers must be nine digits or less:

<div id="example-243" class="example">

<div class="examplenum">

[Example 243](#example-243)

</div>

<div class="column">

    123456789. ok

</div>

<div class="column">

    <ol start="123456789">
    <li>ok</li>
    </ol>

</div>

</div>

<div id="example-244" class="example">

<div class="examplenum">

[Example 244](#example-244)

</div>

<div class="column">

    1234567890. not ok

</div>

<div class="column">

    <p>1234567890. not ok</p>

</div>

</div>

A start number may begin with 0s:

<div id="example-245" class="example">

<div class="examplenum">

[Example 245](#example-245)

</div>

<div class="column">

    0. ok

</div>

<div class="column">

    <ol start="0">
    <li>ok</li>
    </ol>

</div>

</div>

<div id="example-246" class="example">

<div class="examplenum">

[Example 246](#example-246)

</div>

<div class="column">

    003. ok

</div>

<div class="column">

    <ol start="3">
    <li>ok</li>
    </ol>

</div>

</div>

A start number may not be negative:

<div id="example-247" class="example">

<div class="examplenum">

[Example 247](#example-247)

</div>

<div class="column">

    -1. not ok

</div>

<div class="column">

    <p>-1. not ok</p>

</div>

</div>

2.  **Item starting with indented code.** If a sequence of lines *Ls*
    constitute a sequence of blocks *Bs* starting with an indented code
    block, and *M* is a list marker of width *W* followed by one space,
    then the result of prepending *M* and the following space to the
    first line of *Ls*, and indenting subsequent lines of *Ls* by *W +
    1* spaces, is a list item with *Bs* as its contents. If a line is
    empty, then it need not be indented. The type of the list item
    (bullet or ordered) is determined by the type of its list marker. If
    the list item is ordered, then it is also assigned a start number,
    based on the ordered list marker.

An indented code block will have to be indented four spaces beyond the
edge of the region where text will be included in the list item. In the
following case that is 6 spaces:

<div id="example-248" class="example">

<div class="examplenum">

[Example 248](#example-248)

</div>

<div class="column">

    - foo
    
          bar

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <pre><code>bar
    </code></pre>
    </li>
    </ul>

</div>

</div>

And in this case it is 11 spaces:

<div id="example-249" class="example">

<div class="examplenum">

[Example 249](#example-249)

</div>

<div class="column">

``` 
  10.  foo

           bar
```

</div>

<div class="column">

    <ol start="10">
    <li>
    <p>foo</p>
    <pre><code>bar
    </code></pre>
    </li>
    </ol>

</div>

</div>

If the *first* block in the list item is an indented code block, then by
rule \#2, the contents must be indented *one* space after the list
marker:

<div id="example-250" class="example">

<div class="examplenum">

[Example 250](#example-250)

</div>

<div class="column">

``` 
    indented code

paragraph

    more code
```

</div>

<div class="column">

    <pre><code>indented code
    </code></pre>
    <p>paragraph</p>
    <pre><code>more code
    </code></pre>

</div>

</div>

<div id="example-251" class="example">

<div class="examplenum">

[Example 251](#example-251)

</div>

<div class="column">

    1.     indented code
    
       paragraph
    
           more code

</div>

<div class="column">

    <ol>
    <li>
    <pre><code>indented code
    </code></pre>
    <p>paragraph</p>
    <pre><code>more code
    </code></pre>
    </li>
    </ol>

</div>

</div>

Note that an additional space indent is interpreted as space inside the
code block:

<div id="example-252" class="example">

<div class="examplenum">

[Example 252](#example-252)

</div>

<div class="column">

    1.      indented code
    
       paragraph
    
           more code

</div>

<div class="column">

    <ol>
    <li>
    <pre><code> indented code
    </code></pre>
    <p>paragraph</p>
    <pre><code>more code
    </code></pre>
    </li>
    </ol>

</div>

</div>

Note that rules \#1 and \#2 only apply to two cases: (a) cases in which
the lines to be included in a list item begin with a [non-whitespace
character](#non-whitespace-character), and (b) cases in which they begin
with an indented code block. In a case like the following, where the
first block begins with a three-space indent, the rules do not allow us
to form a list item by indenting the whole thing and prepending a list
marker:

<div id="example-253" class="example">

<div class="examplenum">

[Example 253](#example-253)

</div>

<div class="column">

``` 
   foo

bar
```

</div>

<div class="column">

    <p>foo</p>
    <p>bar</p>

</div>

</div>

<div id="example-254" class="example">

<div class="examplenum">

[Example 254](#example-254)

</div>

<div class="column">

    -    foo
    
      bar

</div>

<div class="column">

    <ul>
    <li>foo</li>
    </ul>
    <p>bar</p>

</div>

</div>

This is not a significant restriction, because when a block begins with
1-3 spaces indent, the indentation can always be removed without a
change in interpretation, allowing rule \#1 to be applied. So, in the
above case:

<div id="example-255" class="example">

<div class="examplenum">

[Example 255](#example-255)

</div>

<div class="column">

    -  foo
    
       bar

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <p>bar</p>
    </li>
    </ul>

</div>

</div>

3.  **Item starting with a blank line.** If a sequence of lines *Ls*
    starting with a single [blank line](#blank-line) constitute a
    (possibly empty) sequence of blocks *Bs*, not separated from each
    other by more than one blank line, and *M* is a list marker of width
    *W*, then the result of prepending *M* to the first line of *Ls*,
    and indenting subsequent lines of *Ls* by *W + 1* spaces, is a list
    item with *Bs* as its contents. If a line is empty, then it need not
    be indented. The type of the list item (bullet or ordered) is
    determined by the type of its list marker. If the list item is
    ordered, then it is also assigned a start number, based on the
    ordered list marker.

Here are some list items that start with a blank line but are not empty:

<div id="example-256" class="example">

<div class="examplenum">

[Example 256](#example-256)

</div>

<div class="column">

    -
      foo
    -
      ```
      bar
      ```
    -
          baz

</div>

<div class="column">

    <ul>
    <li>foo</li>
    <li>
    <pre><code>bar
    </code></pre>
    </li>
    <li>
    <pre><code>baz
    </code></pre>
    </li>
    </ul>

</div>

</div>

When the list item starts with a blank line, the number of spaces
following the list marker doesn’t change the required indentation:

<div id="example-257" class="example">

<div class="examplenum">

[Example 257](#example-257)

</div>

<div class="column">

    -   
      foo

</div>

<div class="column">

    <ul>
    <li>foo</li>
    </ul>

</div>

</div>

A list item can begin with at most one blank line. In the following
example, `foo` is not part of the list item:

<div id="example-258" class="example">

<div class="examplenum">

[Example 258](#example-258)

</div>

<div class="column">

    -
    
      foo

</div>

<div class="column">

    <ul>
    <li></li>
    </ul>
    <p>foo</p>

</div>

</div>

Here is an empty bullet list item:

<div id="example-259" class="example">

<div class="examplenum">

[Example 259](#example-259)

</div>

<div class="column">

    - foo
    -
    - bar

</div>

<div class="column">

    <ul>
    <li>foo</li>
    <li></li>
    <li>bar</li>
    </ul>

</div>

</div>

It does not matter whether there are spaces following the [list
marker](#list-marker):

<div id="example-260" class="example">

<div class="examplenum">

[Example 260](#example-260)

</div>

<div class="column">

    - foo
    -   
    - bar

</div>

<div class="column">

    <ul>
    <li>foo</li>
    <li></li>
    <li>bar</li>
    </ul>

</div>

</div>

Here is an empty ordered list item:

<div id="example-261" class="example">

<div class="examplenum">

[Example 261](#example-261)

</div>

<div class="column">

    1. foo
    2.
    3. bar

</div>

<div class="column">

    <ol>
    <li>foo</li>
    <li></li>
    <li>bar</li>
    </ol>

</div>

</div>

A list may start or end with an empty list item:

<div id="example-262" class="example">

<div class="examplenum">

[Example 262](#example-262)

</div>

<div class="column">

``` 
*
```

</div>

<div class="column">

    <ul>
    <li></li>
    </ul>

</div>

</div>

However, an empty list item cannot interrupt a paragraph:

<div id="example-263" class="example">

<div class="examplenum">

[Example 263](#example-263)

</div>

<div class="column">

    foo
    *
    
    foo
    1.

</div>

<div class="column">

    <p>foo
    *</p>
    <p>foo
    1.</p>

</div>

</div>

4.  **Indentation.** If a sequence of lines *Ls* constitutes a list item
    according to rule \#1, \#2, or \#3, then the result of indenting
    each line of *Ls* by 1-3 spaces (the same for each line) also
    constitutes a list item with the same contents and attributes. If a
    line is empty, then it need not be indented.

Indented one space:

<div id="example-264" class="example">

<div class="examplenum">

[Example 264](#example-264)

</div>

<div class="column">

``` 
 1.  A paragraph
     with two lines.

         indented code

     > A block quote.
```

</div>

<div class="column">

    <ol>
    <li>
    <p>A paragraph
    with two lines.</p>
    <pre><code>indented code
    </code></pre>
    <blockquote>
    <p>A block quote.</p>
    </blockquote>
    </li>
    </ol>

</div>

</div>

Indented two spaces:

<div id="example-265" class="example">

<div class="examplenum">

[Example 265](#example-265)

</div>

<div class="column">

``` 
  1.  A paragraph
      with two lines.

          indented code

      > A block quote.
```

</div>

<div class="column">

    <ol>
    <li>
    <p>A paragraph
    with two lines.</p>
    <pre><code>indented code
    </code></pre>
    <blockquote>
    <p>A block quote.</p>
    </blockquote>
    </li>
    </ol>

</div>

</div>

Indented three spaces:

<div id="example-266" class="example">

<div class="examplenum">

[Example 266](#example-266)

</div>

<div class="column">

``` 
   1.  A paragraph
       with two lines.

           indented code

       > A block quote.
```

</div>

<div class="column">

    <ol>
    <li>
    <p>A paragraph
    with two lines.</p>
    <pre><code>indented code
    </code></pre>
    <blockquote>
    <p>A block quote.</p>
    </blockquote>
    </li>
    </ol>

</div>

</div>

Four spaces indent gives a code block:

<div id="example-267" class="example">

<div class="examplenum">

[Example 267](#example-267)

</div>

<div class="column">

``` 
    1.  A paragraph
        with two lines.

            indented code

        > A block quote.
```

</div>

<div class="column">

    <pre><code>1.  A paragraph
        with two lines.
    
            indented code
    
        &gt; A block quote.
    </code></pre>

</div>

</div>

5.  **Laziness.** If a string of lines *Ls* constitute a [list
    item](#list-items) with contents *Bs*, then the result of deleting
    some or all of the indentation from one or more lines in which the
    next [non-whitespace character](#non-whitespace-character) after the
    indentation is [paragraph continuation
    text](#paragraph-continuation-text) is a list item with the same
    contents and attributes. The unindented lines are called [lazy
    continuation line](#lazy-continuation-line)s.

Here is an example with [lazy continuation
lines](#lazy-continuation-line):

<div id="example-268" class="example">

<div class="examplenum">

[Example 268](#example-268)

</div>

<div class="column">

``` 
  1.  A paragraph
with two lines.

          indented code

      > A block quote.
```

</div>

<div class="column">

    <ol>
    <li>
    <p>A paragraph
    with two lines.</p>
    <pre><code>indented code
    </code></pre>
    <blockquote>
    <p>A block quote.</p>
    </blockquote>
    </li>
    </ol>

</div>

</div>

Indentation can be partially deleted:

<div id="example-269" class="example">

<div class="examplenum">

[Example 269](#example-269)

</div>

<div class="column">

``` 
  1.  A paragraph
    with two lines.
```

</div>

<div class="column">

    <ol>
    <li>A paragraph
    with two lines.</li>
    </ol>

</div>

</div>

These examples show how laziness can work in nested structures:

<div id="example-270" class="example">

<div class="examplenum">

[Example 270](#example-270)

</div>

<div class="column">

    > 1. > Blockquote
    continued here.

</div>

<div class="column">

    <blockquote>
    <ol>
    <li>
    <blockquote>
    <p>Blockquote
    continued here.</p>
    </blockquote>
    </li>
    </ol>
    </blockquote>

</div>

</div>

<div id="example-271" class="example">

<div class="examplenum">

[Example 271](#example-271)

</div>

<div class="column">

    > 1. > Blockquote
    > continued here.

</div>

<div class="column">

    <blockquote>
    <ol>
    <li>
    <blockquote>
    <p>Blockquote
    continued here.</p>
    </blockquote>
    </li>
    </ol>
    </blockquote>

</div>

</div>

6.  **That’s all.** Nothing that is not counted as a list item by rules
    \#1–5 counts as a [list item](#list-items).

The rules for sublists follow from the general rules
[above](#list-items). A sublist must be indented the same number of
spaces a paragraph would need to be in order to be included in the list
item.

So, in this case we need two spaces indent:

<div id="example-272" class="example">

<div class="examplenum">

[Example 272](#example-272)

</div>

<div class="column">

    - foo
      - bar
        - baz
          - boo

</div>

<div class="column">

    <ul>
    <li>foo
    <ul>
    <li>bar
    <ul>
    <li>baz
    <ul>
    <li>boo</li>
    </ul>
    </li>
    </ul>
    </li>
    </ul>
    </li>
    </ul>

</div>

</div>

One is not enough:

<div id="example-273" class="example">

<div class="examplenum">

[Example 273](#example-273)

</div>

<div class="column">

    - foo
     - bar
      - baz
       - boo

</div>

<div class="column">

    <ul>
    <li>foo</li>
    <li>bar</li>
    <li>baz</li>
    <li>boo</li>
    </ul>

</div>

</div>

Here we need four, because the list marker is wider:

<div id="example-274" class="example">

<div class="examplenum">

[Example 274](#example-274)

</div>

<div class="column">

    10) foo
        - bar

</div>

<div class="column">

    <ol start="10">
    <li>foo
    <ul>
    <li>bar</li>
    </ul>
    </li>
    </ol>

</div>

</div>

Three is not enough:

<div id="example-275" class="example">

<div class="examplenum">

[Example 275](#example-275)

</div>

<div class="column">

    10) foo
       - bar

</div>

<div class="column">

    <ol start="10">
    <li>foo</li>
    </ol>
    <ul>
    <li>bar</li>
    </ul>

</div>

</div>

A list may be the first block in a list item:

<div id="example-276" class="example">

<div class="examplenum">

[Example 276](#example-276)

</div>

<div class="column">

    - - foo

</div>

<div class="column">

    <ul>
    <li>
    <ul>
    <li>foo</li>
    </ul>
    </li>
    </ul>

</div>

</div>

<div id="example-277" class="example">

<div class="examplenum">

[Example 277](#example-277)

</div>

<div class="column">

    1. - 2. foo

</div>

<div class="column">

    <ol>
    <li>
    <ul>
    <li>
    <ol start="2">
    <li>foo</li>
    </ol>
    </li>
    </ul>
    </li>
    </ol>

</div>

</div>

A list item can contain a heading:

<div id="example-278" class="example">

<div class="examplenum">

[Example 278](#example-278)

</div>

<div class="column">

    - # Foo
    - Bar
      ---
      baz

</div>

<div class="column">

    <ul>
    <li>
    <h1>Foo</h1>
    </li>
    <li>
    <h2>Bar</h2>
    baz</li>
    </ul>

</div>

</div>

### <span class="number">5.2.1</span>Motivation

John Gruber’s Markdown spec says the following about list items:

1.  “List markers typically start at the left margin, but may be
    indented by up to three spaces. List markers must be followed by one
    or more spaces or a tab.”

2.  “To make lists look nice, you can wrap items with hanging indents….
    But if you don’t want to, you don’t have to.”

3.  “List items may consist of multiple paragraphs. Each subsequent
    paragraph in a list item must be indented by either 4 spaces or one
    tab.”

4.  “It looks nice if you indent every line of the subsequent
    paragraphs, but here again, Markdown will allow you to be lazy.”

5.  “To put a blockquote within a list item, the blockquote’s `>`
    delimiters need to be indented.”

6.  “To put a code block within a list item, the code block needs to be
    indented twice — 8 spaces or two tabs.”

These rules specify that a paragraph under a list item must be indented
four spaces (presumably, from the left margin, rather than the start of
the list marker, but this is not said), and that code under a list item
must be indented eight spaces instead of the usual four. They also say
that a block quote must be indented, but not by how much; however, the
example given has four spaces indentation. Although nothing is said
about other kinds of block-level content, it is certainly reasonable to
infer that *all* block elements under a list item, including other
lists, must be indented four spaces. This principle has been called the
*four-space rule*.

The four-space rule is clear and principled, and if the reference
implementation `Markdown.pl` had followed it, it probably would have
become the standard. However, `Markdown.pl` allowed paragraphs and
sublists to start with only two spaces indentation, at least on the
outer level. Worse, its behavior was inconsistent: a sublist of an
outer-level list needed two spaces indentation, but a sublist of this
sublist needed three spaces. It is not surprising, then, that different
implementations of Markdown have developed very different rules for
determining what comes under a list item. (Pandoc and python-Markdown,
for example, stuck with Gruber’s syntax description and the four-space
rule, while discount, redcarpet, marked, PHP Markdown, and others
followed `Markdown.pl`’s behavior more closely.)

Unfortunately, given the divergences between implementations, there is
no way to give a spec for list items that will be guaranteed not to
break any existing documents. However, the spec given here should
correctly handle lists formatted with either the four-space rule or the
more forgiving `Markdown.pl` behavior, provided they are laid out in a
way that is natural for a human to read.

The strategy here is to let the width and indentation of the list marker
determine the indentation necessary for blocks to fall under the list
item, rather than having a fixed and arbitrary number. The writer can
think of the body of the list item as a unit which gets indented to the
right enough to fit the list marker (and any indentation on the list
marker). (The laziness rule, \#5, then allows continuation lines to be
unindented if needed.)

This rule is superior, we claim, to any rule requiring a fixed level of
indentation from the margin. The four-space rule is clear but unnatural.
It is quite unintuitive that

    - foo
    
      bar
    
      - baz

should be parsed as two lists with an intervening paragraph,

    <ul>
    <li>foo</li>
    </ul>
    <p>bar</p>
    <ul>
    <li>baz</li>
    </ul>

as the four-space rule demands, rather than a single list,

    <ul>
    <li>
    <p>foo</p>
    <p>bar</p>
    <ul>
    <li>baz</li>
    </ul>
    </li>
    </ul>

The choice of four spaces is arbitrary. It can be learned, but it is not
likely to be guessed, and it trips up beginners regularly.

Would it help to adopt a two-space rule? The problem is that such a
rule, together with the rule allowing 1–3 spaces indentation of the
initial list marker, allows text that is indented *less than* the
original list marker to be included in the list item. For example,
`Markdown.pl` parses

``` 
   - one

  two
```

as a single list item, with `two` a continuation paragraph:

    <ul>
    <li>
    <p>one</p>
    <p>two</p>
    </li>
    </ul>

and similarly

    >   - one
    >
    >  two

as

    <blockquote>
    <ul>
    <li>
    <p>one</p>
    <p>two</p>
    </li>
    </ul>
    </blockquote>

This is extremely unintuitive.

Rather than requiring a fixed indent from the margin, we could require a
fixed indent (say, two spaces, or even one space) from the list marker
(which may itself be indented). This proposal would remove the last
anomaly discussed. Unlike the spec presented above, it would count the
following as a list item with a subparagraph, even though the paragraph
`bar` is not indented as far as the first paragraph `foo`:

``` 
 10. foo

   bar  
```

Arguably this text does read like a list item with `bar` as a
subparagraph, which may count in favor of the proposal. However, on this
proposal indented code would have to be indented six spaces after the
list marker. And this would break a lot of existing Markdown, which has
the pattern:

    1.  foo
    
            indented code

where the code is indented eight spaces. The spec above, by contrast,
will parse this text as expected, since the code block’s indentation is
measured from the beginning of `foo`.

The one case that needs special treatment is a list item that *starts*
with indented code. How much indentation is required in that case, since
we don’t have a “first paragraph” to measure from? Rule \#2 simply
stipulates that in such cases, we require one space indentation from the
list marker (and then the normal four spaces for the indented code).
This will match the four-space rule in cases where the list marker plus
its initial indentation takes four spaces (a common case), but diverge
in other cases.

<div class="extension">

## [](#TOC)<span class="number">5.3</span>Task list items (extension)

GFM enables the `tasklist` extension, where an additional processing
step is performed on [list items](#list-items).

A [task list item](#task-list-item) is a [list item](#list-items) where
the first block in it is a paragraph which begins with a [task list item
marker](#task-list-item-marker) and at least one whitespace character
before any other content.

A [task list item marker](#task-list-item-marker) consists of an
optional number of spaces, a left bracket (`[`), either a whitespace
character or the letter `x` in either lowercase or uppercase, and then a
right bracket (`]`).

When rendered, the [task list item marker](#task-list-item-marker) is
replaced with a semantic checkbox element; in an HTML output, this would
be an `<input type="checkbox">` element.

If the character between the brackets is a whitespace character, the
checkbox is unchecked. Otherwise, the checkbox is checked.

This spec does not define how the checkbox elements are interacted with:
in practice, implementors are free to render the checkboxes as disabled
or inmutable elements, or they may dynamically handle dynamic
interactions (i.e. checking, unchecking) in the final rendered document.

<div id="example-279" class="example">

<div class="examplenum">

[Example 279](#example-279)

</div>

<div class="column">

    - [ ] foo
    - [x] bar

</div>

<div class="column">

    <ul>
    <li><input disabled="" type="checkbox"> foo</li>
    <li><input checked="" disabled="" type="checkbox"> bar</li>
    </ul>

</div>

</div>

Task lists can be arbitrarily nested:

<div id="example-280" class="example">

<div class="examplenum">

[Example 280](#example-280)

</div>

<div class="column">

    - [x] foo
      - [ ] bar
      - [x] baz
    - [ ] bim

</div>

<div class="column">

    <ul>
    <li><input checked="" disabled="" type="checkbox"> foo
    <ul>
    <li><input disabled="" type="checkbox"> bar</li>
    <li><input checked="" disabled="" type="checkbox"> baz</li>
    </ul>
    </li>
    <li><input disabled="" type="checkbox"> bim</li>
    </ul>

</div>

</div>

</div>

## [](#TOC)<span class="number">5.4</span>Lists

A [list](#list) is a sequence of one or more list items [of the same
type](#of-the-same-type). The list items may be separated by any number
of blank lines.

Two list items are [of the same type](#of-the-same-type) if they begin
with a [list marker](#list-marker) of the same type. Two list markers
are of the same type if (a) they are bullet list markers using the same
character (`-`, `+`, or `*`) or (b) they are ordered list numbers with
the same delimiter (either `.` or `)`).

A list is an [ordered list](#ordered-list) if its constituent list items
begin with [ordered list markers](#ordered-list-marker), and a [bullet
list](#bullet-list) if its constituent list items begin with [bullet
list markers](#bullet-list-marker).

The [start number](#start-number) of an [ordered list](#ordered-list) is
determined by the list number of its initial list item. The numbers of
subsequent list items are disregarded.

A list is [loose](#loose) if any of its constituent list items are
separated by blank lines, or if any of its constituent list items
directly contain two block-level elements with a blank line between
them. Otherwise a list is [tight](#tight). (The difference in HTML
output is that paragraphs in a loose list are wrapped in `<p>` tags,
while paragraphs in a tight list are not.)

Changing the bullet or ordered list delimiter starts a new list:

<div id="example-281" class="example">

<div class="examplenum">

[Example 281](#example-281)

</div>

<div class="column">

    - foo
    - bar
    + baz

</div>

<div class="column">

    <ul>
    <li>foo</li>
    <li>bar</li>
    </ul>
    <ul>
    <li>baz</li>
    </ul>

</div>

</div>

<div id="example-282" class="example">

<div class="examplenum">

[Example 282](#example-282)

</div>

<div class="column">

    1. foo
    2. bar
    3) baz

</div>

<div class="column">

    <ol>
    <li>foo</li>
    <li>bar</li>
    </ol>
    <ol start="3">
    <li>baz</li>
    </ol>

</div>

</div>

In CommonMark, a list can interrupt a paragraph. That is, no blank line
is needed to separate a paragraph from a following list:

<div id="example-283" class="example">

<div class="examplenum">

[Example 283](#example-283)

</div>

<div class="column">

    Foo
    - bar
    - baz

</div>

<div class="column">

    <p>Foo</p>
    <ul>
    <li>bar</li>
    <li>baz</li>
    </ul>

</div>

</div>

`Markdown.pl` does not allow this, through fear of triggering a list via
a numeral in a hard-wrapped line:

    The number of windows in my house is
    14.  The number of doors is 6.

Oddly, though, `Markdown.pl` *does* allow a blockquote to interrupt a
paragraph, even though the same considerations might apply.

In CommonMark, we do allow lists to interrupt paragraphs, for two
reasons. First, it is natural and not uncommon for people to start lists
without blank lines:

    I need to buy
    - new shoes
    - a coat
    - a plane ticket

Second, we are attracted to a

> [principle of uniformity](#principle-of-uniformity): if a chunk of
> text has a certain meaning, it will continue to have the same meaning
> when put into a container block (such as a list item or blockquote).

(Indeed, the spec for [list items](#list-items) and [block
quotes](#block-quotes) presupposes this principle.) This principle
implies that if

``` 
  * I need to buy
    - new shoes
    - a coat
    - a plane ticket
```

is a list item containing a paragraph followed by a nested sublist, as
all Markdown implementations agree it is (though the paragraph may be
rendered without `<p>` tags, since the list is “tight”), then

    I need to buy
    - new shoes
    - a coat
    - a plane ticket

by itself should be a paragraph followed by a nested sublist.

Since it is well established Markdown practice to allow lists to
interrupt paragraphs inside list items, the [principle of
uniformity](#principle-of-uniformity) requires us to allow this outside
list items as well.
([reStructuredText](http://docutils.sourceforge.net/rst.html) takes a
different approach, requiring blank lines before lists even inside other
list items.)

In order to solve of unwanted lists in paragraphs with hard-wrapped
numerals, we allow only lists starting with `1` to interrupt paragraphs.
Thus,

<div id="example-284" class="example">

<div class="examplenum">

[Example 284](#example-284)

</div>

<div class="column">

    The number of windows in my house is
    14.  The number of doors is 6.

</div>

<div class="column">

    <p>The number of windows in my house is
    14.  The number of doors is 6.</p>

</div>

</div>

We may still get an unintended result in cases like

<div id="example-285" class="example">

<div class="examplenum">

[Example 285](#example-285)

</div>

<div class="column">

    The number of windows in my house is
    1.  The number of doors is 6.

</div>

<div class="column">

    <p>The number of windows in my house is</p>
    <ol>
    <li>The number of doors is 6.</li>
    </ol>

</div>

</div>

but this rule should prevent most spurious list captures.

There can be any number of blank lines between items:

<div id="example-286" class="example">

<div class="examplenum">

[Example 286](#example-286)

</div>

<div class="column">

    - foo
    
    - bar
    
    
    - baz

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    </li>
    <li>
    <p>bar</p>
    </li>
    <li>
    <p>baz</p>
    </li>
    </ul>

</div>

</div>

<div id="example-287" class="example">

<div class="examplenum">

[Example 287](#example-287)

</div>

<div class="column">

    - foo
      - bar
        - baz
    
    
          bim

</div>

<div class="column">

    <ul>
    <li>foo
    <ul>
    <li>bar
    <ul>
    <li>
    <p>baz</p>
    <p>bim</p>
    </li>
    </ul>
    </li>
    </ul>
    </li>
    </ul>

</div>

</div>

To separate consecutive lists of the same type, or to separate a list
from an indented code block that would otherwise be parsed as a
subparagraph of the final list item, you can insert a blank HTML
comment:

<div id="example-288" class="example">

<div class="examplenum">

[Example 288](#example-288)

</div>

<div class="column">

    - foo
    - bar
    
    <!-- -->
    
    - baz
    - bim

</div>

<div class="column">

    <ul>
    <li>foo</li>
    <li>bar</li>
    </ul>
    <!-- -->
    <ul>
    <li>baz</li>
    <li>bim</li>
    </ul>

</div>

</div>

<div id="example-289" class="example">

<div class="examplenum">

[Example 289](#example-289)

</div>

<div class="column">

    -   foo
    
        notcode
    
    -   foo
    
    <!-- -->
    
        code

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <p>notcode</p>
    </li>
    <li>
    <p>foo</p>
    </li>
    </ul>
    <!-- -->
    <pre><code>code
    </code></pre>

</div>

</div>

List items need not be indented to the same level. The following list
items will be treated as items at the same list level, since none is
indented enough to belong to the previous list item:

<div id="example-290" class="example">

<div class="examplenum">

[Example 290](#example-290)

</div>

<div class="column">

    - a
     - b
      - c
       - d
      - e
     - f
    - g

</div>

<div class="column">

    <ul>
    <li>a</li>
    <li>b</li>
    <li>c</li>
    <li>d</li>
    <li>e</li>
    <li>f</li>
    <li>g</li>
    </ul>

</div>

</div>

<div id="example-291" class="example">

<div class="examplenum">

[Example 291](#example-291)

</div>

<div class="column">

    1. a
    
      2. b
    
       3. c

</div>

<div class="column">

    <ol>
    <li>
    <p>a</p>
    </li>
    <li>
    <p>b</p>
    </li>
    <li>
    <p>c</p>
    </li>
    </ol>

</div>

</div>

Note, however, that list items may not be indented more than three
spaces. Here `- e` is treated as a paragraph continuation line, because
it is indented more than three spaces:

<div id="example-292" class="example">

<div class="examplenum">

[Example 292](#example-292)

</div>

<div class="column">

    - a
     - b
      - c
       - d
        - e

</div>

<div class="column">

    <ul>
    <li>a</li>
    <li>b</li>
    <li>c</li>
    <li>d
    - e</li>
    </ul>

</div>

</div>

And here, `3. c` is treated as in indented code block, because it is
indented four spaces and preceded by a blank line.

<div id="example-293" class="example">

<div class="examplenum">

[Example 293](#example-293)

</div>

<div class="column">

    1. a
    
      2. b
    
        3. c

</div>

<div class="column">

    <ol>
    <li>
    <p>a</p>
    </li>
    <li>
    <p>b</p>
    </li>
    </ol>
    <pre><code>3. c
    </code></pre>

</div>

</div>

This is a loose list, because there is a blank line between two of the
list items:

<div id="example-294" class="example">

<div class="examplenum">

[Example 294](#example-294)

</div>

<div class="column">

    - a
    - b
    
    - c

</div>

<div class="column">

    <ul>
    <li>
    <p>a</p>
    </li>
    <li>
    <p>b</p>
    </li>
    <li>
    <p>c</p>
    </li>
    </ul>

</div>

</div>

So is this, with a empty second item:

<div id="example-295" class="example">

<div class="examplenum">

[Example 295](#example-295)

</div>

<div class="column">

    * a
    *
    
    * c

</div>

<div class="column">

    <ul>
    <li>
    <p>a</p>
    </li>
    <li></li>
    <li>
    <p>c</p>
    </li>
    </ul>

</div>

</div>

These are loose lists, even though there is no space between the items,
because one of the items directly contains two block-level elements with
a blank line between them:

<div id="example-296" class="example">

<div class="examplenum">

[Example 296](#example-296)

</div>

<div class="column">

    - a
    - b
    
      c
    - d

</div>

<div class="column">

    <ul>
    <li>
    <p>a</p>
    </li>
    <li>
    <p>b</p>
    <p>c</p>
    </li>
    <li>
    <p>d</p>
    </li>
    </ul>

</div>

</div>

<div id="example-297" class="example">

<div class="examplenum">

[Example 297](#example-297)

</div>

<div class="column">

    - a
    - b
    
      [ref]: /url
    - d

</div>

<div class="column">

    <ul>
    <li>
    <p>a</p>
    </li>
    <li>
    <p>b</p>
    </li>
    <li>
    <p>d</p>
    </li>
    </ul>

</div>

</div>

This is a tight list, because the blank lines are in a code block:

<div id="example-298" class="example">

<div class="examplenum">

[Example 298](#example-298)

</div>

<div class="column">

    - a
    - ```
      b
    
    
      ```
    - c

</div>

<div class="column">

    <ul>
    <li>a</li>
    <li>
    <pre><code>b
    
    
    </code></pre>
    </li>
    <li>c</li>
    </ul>

</div>

</div>

This is a tight list, because the blank line is between two paragraphs
of a sublist. So the sublist is loose while the outer list is tight:

<div id="example-299" class="example">

<div class="examplenum">

[Example 299](#example-299)

</div>

<div class="column">

    - a
      - b
    
        c
    - d

</div>

<div class="column">

    <ul>
    <li>a
    <ul>
    <li>
    <p>b</p>
    <p>c</p>
    </li>
    </ul>
    </li>
    <li>d</li>
    </ul>

</div>

</div>

This is a tight list, because the blank line is inside the block quote:

<div id="example-300" class="example">

<div class="examplenum">

[Example 300](#example-300)

</div>

<div class="column">

    * a
      > b
      >
    * c

</div>

<div class="column">

    <ul>
    <li>a
    <blockquote>
    <p>b</p>
    </blockquote>
    </li>
    <li>c</li>
    </ul>

</div>

</div>

This list is tight, because the consecutive block elements are not
separated by blank lines:

<div id="example-301" class="example">

<div class="examplenum">

[Example 301](#example-301)

</div>

<div class="column">

    - a
      > b
      ```
      c
      ```
    - d

</div>

<div class="column">

    <ul>
    <li>a
    <blockquote>
    <p>b</p>
    </blockquote>
    <pre><code>c
    </code></pre>
    </li>
    <li>d</li>
    </ul>

</div>

</div>

A single-paragraph list is tight:

<div id="example-302" class="example">

<div class="examplenum">

[Example 302](#example-302)

</div>

<div class="column">

    - a

</div>

<div class="column">

    <ul>
    <li>a</li>
    </ul>

</div>

</div>

<div id="example-303" class="example">

<div class="examplenum">

[Example 303](#example-303)

</div>

<div class="column">

    - a
      - b

</div>

<div class="column">

    <ul>
    <li>a
    <ul>
    <li>b</li>
    </ul>
    </li>
    </ul>

</div>

</div>

This list is loose, because of the blank line between the two block
elements in the list item:

<div id="example-304" class="example">

<div class="examplenum">

[Example 304](#example-304)

</div>

<div class="column">

    1. ```
       foo
       ```
    
       bar

</div>

<div class="column">

    <ol>
    <li>
    <pre><code>foo
    </code></pre>
    <p>bar</p>
    </li>
    </ol>

</div>

</div>

Here the outer list is loose, the inner list tight:

<div id="example-305" class="example">

<div class="examplenum">

[Example 305](#example-305)

</div>

<div class="column">

    * foo
      * bar
    
      baz

</div>

<div class="column">

    <ul>
    <li>
    <p>foo</p>
    <ul>
    <li>bar</li>
    </ul>
    <p>baz</p>
    </li>
    </ul>

</div>

</div>

<div id="example-306" class="example">

<div class="examplenum">

[Example 306](#example-306)

</div>

<div class="column">

    - a
      - b
      - c
    
    - d
      - e
      - f

</div>

<div class="column">

    <ul>
    <li>
    <p>a</p>
    <ul>
    <li>b</li>
    <li>c</li>
    </ul>
    </li>
    <li>
    <p>d</p>
    <ul>
    <li>e</li>
    <li>f</li>
    </ul>
    </li>
    </ul>

</div>

</div>

# <span class="number">6</span>Inlines

Inlines are parsed sequentially from the beginning of the character
stream to the end (left to right, in left-to-right languages). Thus, for
example, in

<div id="example-307" class="example">

<div class="examplenum">

[Example 307](#example-307)

</div>

<div class="column">

    `hi`lo`

</div>

<div class="column">

    <p><code>hi</code>lo`</p>

</div>

</div>

`hi` is parsed as code, leaving the backtick at the end as a literal
backtick.

## [](#TOC)<span class="number">6.1</span>Backslash escapes

Any ASCII punctuation character may be backslash-escaped:

<div id="example-308" class="example">

<div class="examplenum">

[Example 308](#example-308)

</div>

<div class="column">

    \!\"\#\$\%\&\'\(\)\*\+\,\-\.\/\:\;\<\=\>\?\@\[\\\]\^\_\`\{\|\}\~

</div>

<div class="column">

    <p>!&quot;#$%&amp;'()*+,-./:;&lt;=&gt;?@[\]^_`{|}~</p>

</div>

</div>

Backslashes before other characters are treated as literal backslashes:

<div id="example-309" class="example">

<div class="examplenum">

[Example 309](#example-309)

</div>

<div class="column">

    \→\A\a\ \3\φ\«

</div>

<div class="column">

    <p>\→\A\a\ \3\φ\«</p>

</div>

</div>

Escaped characters are treated as regular characters and do not have
their usual Markdown meanings:

<div id="example-310" class="example">

<div class="examplenum">

[Example 310](#example-310)

</div>

<div class="column">

    \*not emphasized*
    \<br/> not a tag
    \[not a link](/foo)
    \`not code`
    1\. not a list
    \* not a list
    \# not a heading
    \[foo]: /url "not a reference"
    \&ouml; not a character entity

</div>

<div class="column">

    <p>*not emphasized*
    &lt;br/&gt; not a tag
    [not a link](/foo)
    `not code`
    1. not a list
    * not a list
    # not a heading
    [foo]: /url &quot;not a reference&quot;
    &amp;ouml; not a character entity</p>

</div>

</div>

If a backslash is itself escaped, the following character is not:

<div id="example-311" class="example">

<div class="examplenum">

[Example 311](#example-311)

</div>

<div class="column">

    \\*emphasis*

</div>

<div class="column">

    <p>\<em>emphasis</em></p>

</div>

</div>

A backslash at the end of the line is a [hard line
break](#hard-line-break):

<div id="example-312" class="example">

<div class="examplenum">

[Example 312](#example-312)

</div>

<div class="column">

    foo\
    bar

</div>

<div class="column">

    <p>foo<br />
    bar</p>

</div>

</div>

Backslash escapes do not work in code blocks, code spans, autolinks, or
raw HTML:

<div id="example-313" class="example">

<div class="examplenum">

[Example 313](#example-313)

</div>

<div class="column">

    `` \[\` ``

</div>

<div class="column">

    <p><code>\[\`</code></p>

</div>

</div>

<div id="example-314" class="example">

<div class="examplenum">

[Example 314](#example-314)

</div>

<div class="column">

``` 
    \[\]
```

</div>

<div class="column">

    <pre><code>\[\]
    </code></pre>

</div>

</div>

<div id="example-315" class="example">

<div class="examplenum">

[Example 315](#example-315)

</div>

<div class="column">

    ~~~
    \[\]
    ~~~

</div>

<div class="column">

    <pre><code>\[\]
    </code></pre>

</div>

</div>

<div id="example-316" class="example">

<div class="examplenum">

[Example 316](#example-316)

</div>

<div class="column">

    <http://example.com?find=\*>

</div>

<div class="column">

    <p><a href="http://example.com?find=%5C*">http://example.com?find=\*</a></p>

</div>

</div>

<div id="example-317" class="example">

<div class="examplenum">

[Example 317](#example-317)

</div>

<div class="column">

    <a href="/bar\/)">

</div>

<div class="column">

    <a href="/bar\/)">

</div>

</div>

But they work in all other contexts, including URLs and link titles,
link references, and [info strings](#info-string) in [fenced code
blocks](#fenced-code-blocks):

<div id="example-318" class="example">

<div class="examplenum">

[Example 318](#example-318)

</div>

<div class="column">

    [foo](/bar\* "ti\*tle")

</div>

<div class="column">

    <p><a href="/bar*" title="ti*tle">foo</a></p>

</div>

</div>

<div id="example-319" class="example">

<div class="examplenum">

[Example 319](#example-319)

</div>

<div class="column">

    [foo]
    
    [foo]: /bar\* "ti\*tle"

</div>

<div class="column">

    <p><a href="/bar*" title="ti*tle">foo</a></p>

</div>

</div>

<div id="example-320" class="example">

<div class="examplenum">

[Example 320](#example-320)

</div>

<div class="column">

    ``` foo\+bar
    foo
    ```

</div>

<div class="column">

    <pre><code class="language-foo+bar">foo
    </code></pre>

</div>

</div>

## [](#TOC)<span class="number">6.2</span>Entity and numeric character references

Valid HTML entity references and numeric character references can be
used in place of the corresponding Unicode character, with the following
exceptions:

  - Entity and character references are not recognized in code blocks
    and code spans.

  - Entity and character references cannot stand in place of special
    characters that define structural elements in CommonMark. For
    example, although `&#42;` can be used in place of a literal `*`
    character, `&#42;` cannot replace `*` in emphasis delimiters, bullet
    list markers, or thematic breaks.

Conforming CommonMark parsers need not store information about whether a
particular character was represented in the source using a Unicode
character or an entity reference.

[Entity references](#entity-references) consist of `&` + any of the
valid HTML5 entity names + `;`. The document
<https://html.spec.whatwg.org/multipage/entities.json> is used as an
authoritative source for the valid entity references and their
corresponding code points.

<div id="example-321" class="example">

<div class="examplenum">

[Example 321](#example-321)

</div>

<div class="column">

    &nbsp; &amp; &copy; &AElig; &Dcaron;
    &frac34; &HilbertSpace; &DifferentialD;
    &ClockwiseContourIntegral; &ngE;

</div>

<div class="column">

    <p>  &amp; © Æ Ď
    ¾ ℋ ⅆ
    ∲ ≧̸</p>

</div>

</div>

[Decimal numeric character
references](#decimal-numeric-character-references) consist of `&#` + a
string of 1–7 arabic digits + `;`. A numeric character reference is
parsed as the corresponding Unicode character. Invalid Unicode code
points will be replaced by the REPLACEMENT CHARACTER (`U+FFFD`). For
security reasons, the code point `U+0000` will also be replaced by
`U+FFFD`.

<div id="example-322" class="example">

<div class="examplenum">

[Example 322](#example-322)

</div>

<div class="column">

    &#35; &#1234; &#992; &#0;

</div>

<div class="column">

    <p># Ӓ Ϡ �</p>

</div>

</div>

[Hexadecimal numeric character
references](#hexadecimal-numeric-character-references) consist of `&#` +
either `X` or `x` + a string of 1-6 hexadecimal digits + `;`. They too
are parsed as the corresponding Unicode character (this time specified
with a hexadecimal numeral instead of decimal).

<div id="example-323" class="example">

<div class="examplenum">

[Example 323](#example-323)

</div>

<div class="column">

    &#X22; &#XD06; &#xcab;

</div>

<div class="column">

    <p>&quot; ആ ಫ</p>

</div>

</div>

Here are some nonentities:

<div id="example-324" class="example">

<div class="examplenum">

[Example 324](#example-324)

</div>

<div class="column">

    &nbsp &x; &#; &#x;
    &#87654321;
    &#abcdef0;
    &ThisIsNotDefined; &hi?;

</div>

<div class="column">

    <p>&amp;nbsp &amp;x; &amp;#; &amp;#x;
    &amp;#87654321;
    &amp;#abcdef0;
    &amp;ThisIsNotDefined; &amp;hi?;</p>

</div>

</div>

Although HTML5 does accept some entity references without a trailing
semicolon (such as `&copy`), these are not recognized here, because it
makes the grammar too ambiguous:

<div id="example-325" class="example">

<div class="examplenum">

[Example 325](#example-325)

</div>

<div class="column">

    &copy

</div>

<div class="column">

    <p>&amp;copy</p>

</div>

</div>

Strings that are not on the list of HTML5 named entities are not
recognized as entity references either:

<div id="example-326" class="example">

<div class="examplenum">

[Example 326](#example-326)

</div>

<div class="column">

    &MadeUpEntity;

</div>

<div class="column">

    <p>&amp;MadeUpEntity;</p>

</div>

</div>

Entity and numeric character references are recognized in any context
besides code spans or code blocks, including URLs, [link
titles](#link-title), and [fenced code block](#fenced-code-block) [info
strings](#info-string):

<div id="example-327" class="example">

<div class="examplenum">

[Example 327](#example-327)

</div>

<div class="column">

    <a href="&ouml;&ouml;.html">

</div>

<div class="column">

    <a href="&ouml;&ouml;.html">

</div>

</div>

<div id="example-328" class="example">

<div class="examplenum">

[Example 328](#example-328)

</div>

<div class="column">

    [foo](/f&ouml;&ouml; "f&ouml;&ouml;")

</div>

<div class="column">

    <p><a href="/f%C3%B6%C3%B6" title="föö">foo</a></p>

</div>

</div>

<div id="example-329" class="example">

<div class="examplenum">

[Example 329](#example-329)

</div>

<div class="column">

    [foo]
    
    [foo]: /f&ouml;&ouml; "f&ouml;&ouml;"

</div>

<div class="column">

    <p><a href="/f%C3%B6%C3%B6" title="föö">foo</a></p>

</div>

</div>

<div id="example-330" class="example">

<div class="examplenum">

[Example 330](#example-330)

</div>

<div class="column">

    ``` f&ouml;&ouml;
    foo
    ```

</div>

<div class="column">

    <pre><code class="language-föö">foo
    </code></pre>

</div>

</div>

Entity and numeric character references are treated as literal text in
code spans and code blocks:

<div id="example-331" class="example">

<div class="examplenum">

[Example 331](#example-331)

</div>

<div class="column">

    `f&ouml;&ouml;`

</div>

<div class="column">

    <p><code>f&amp;ouml;&amp;ouml;</code></p>

</div>

</div>

<div id="example-332" class="example">

<div class="examplenum">

[Example 332](#example-332)

</div>

<div class="column">

``` 
    f&ouml;f&ouml;
```

</div>

<div class="column">

    <pre><code>f&amp;ouml;f&amp;ouml;
    </code></pre>

</div>

</div>

Entity and numeric character references cannot be used in place of
symbols indicating structure in CommonMark documents.

<div id="example-333" class="example">

<div class="examplenum">

[Example 333](#example-333)

</div>

<div class="column">

    &#42;foo&#42;
    *foo*

</div>

<div class="column">

    <p>*foo*
    <em>foo</em></p>

</div>

</div>

<div id="example-334" class="example">

<div class="examplenum">

[Example 334](#example-334)

</div>

<div class="column">

    &#42; foo
    
    * foo

</div>

<div class="column">

    <p>* foo</p>
    <ul>
    <li>foo</li>
    </ul>

</div>

</div>

<div id="example-335" class="example">

<div class="examplenum">

[Example 335](#example-335)

</div>

<div class="column">

    foo&#10;&#10;bar

</div>

<div class="column">

    <p>foo
    
    bar</p>

</div>

</div>

<div id="example-336" class="example">

<div class="examplenum">

[Example 336](#example-336)

</div>

<div class="column">

    &#9;foo

</div>

<div class="column">

    <p>→foo</p>

</div>

</div>

<div id="example-337" class="example">

<div class="examplenum">

[Example 337](#example-337)

</div>

<div class="column">

    [a](url &quot;tit&quot;)

</div>

<div class="column">

    <p>[a](url &quot;tit&quot;)</p>

</div>

</div>

## [](#TOC)<span class="number">6.3</span>Code spans

A [backtick string](#backtick-string) is a string of one or more
backtick characters (`` ` ``) that is neither preceded nor followed by a
backtick.

A [code span](#code-span) begins with a backtick string and ends with a
backtick string of equal length. The contents of the code span are the
characters between the two backtick strings, normalized in the following
ways:

  - First, [line endings](#line-ending) are converted to
    [spaces](#space).
  - If the resulting string both begins *and* ends with a
    [space](#space) character, but does not consist entirely of
    [space](#space) characters, a single [space](#space) character is
    removed from the front and back. This allows you to include code
    that begins or ends with backtick characters, which must be
    separated by whitespace from the opening or closing backtick
    strings.

This is a simple code span:

<div id="example-338" class="example">

<div class="examplenum">

[Example 338](#example-338)

</div>

<div class="column">

    `foo`

</div>

<div class="column">

    <p><code>foo</code></p>

</div>

</div>

Here two backticks are used, because the code contains a backtick. This
example also illustrates stripping of a single leading and trailing
space:

<div id="example-339" class="example">

<div class="examplenum">

[Example 339](#example-339)

</div>

<div class="column">

    `` foo ` bar ``

</div>

<div class="column">

    <p><code>foo ` bar</code></p>

</div>

</div>

This example shows the motivation for stripping leading and trailing
spaces:

<div id="example-340" class="example">

<div class="examplenum">

[Example 340](#example-340)

</div>

<div class="column">

    ` `` `

</div>

<div class="column">

    <p><code>``</code></p>

</div>

</div>

Note that only *one* space is stripped:

<div id="example-341" class="example">

<div class="examplenum">

[Example 341](#example-341)

</div>

<div class="column">

    `  ``  `

</div>

<div class="column">

    <p><code> `` </code></p>

</div>

</div>

The stripping only happens if the space is on both sides of the string:

<div id="example-342" class="example">

<div class="examplenum">

[Example 342](#example-342)

</div>

<div class="column">

    ` a`

</div>

<div class="column">

    <p><code> a</code></p>

</div>

</div>

Only [spaces](#space), and not [unicode whitespace](#unicode-whitespace)
in general, are stripped in this way:

<div id="example-343" class="example">

<div class="examplenum">

[Example 343](#example-343)

</div>

<div class="column">

    ` b `

</div>

<div class="column">

    <p><code> b </code></p>

</div>

</div>

No stripping occurs if the code span contains only spaces:

<div id="example-344" class="example">

<div class="examplenum">

[Example 344](#example-344)

</div>

<div class="column">

    ` `
    `  `

</div>

<div class="column">

    <p><code> </code>
    <code>  </code></p>

</div>

</div>

[Line endings](#line-ending) are treated like spaces:

<div id="example-345" class="example">

<div class="examplenum">

[Example 345](#example-345)

</div>

<div class="column">

    ``
    foo
    bar  
    baz
    ``

</div>

<div class="column">

    <p><code>foo bar   baz</code></p>

</div>

</div>

<div id="example-346" class="example">

<div class="examplenum">

[Example 346](#example-346)

</div>

<div class="column">

    ``
    foo 
    ``

</div>

<div class="column">

    <p><code>foo </code></p>

</div>

</div>

Interior spaces are not collapsed:

<div id="example-347" class="example">

<div class="examplenum">

[Example 347](#example-347)

</div>

<div class="column">

    `foo   bar 
    baz`

</div>

<div class="column">

    <p><code>foo   bar  baz</code></p>

</div>

</div>

Note that browsers will typically collapse consecutive spaces when
rendering `<code>` elements, so it is recommended that the following CSS
be used:

    code{white-space: pre-wrap;}

Note that backslash escapes do not work in code spans. All backslashes
are treated literally:

<div id="example-348" class="example">

<div class="examplenum">

[Example 348](#example-348)

</div>

<div class="column">

    `foo\`bar`

</div>

<div class="column">

    <p><code>foo\</code>bar`</p>

</div>

</div>

Backslash escapes are never needed, because one can always choose a
string of *n* backtick characters as delimiters, where the code does not
contain any strings of exactly *n* backtick characters.

<div id="example-349" class="example">

<div class="examplenum">

[Example 349](#example-349)

</div>

<div class="column">

    ``foo`bar``

</div>

<div class="column">

    <p><code>foo`bar</code></p>

</div>

</div>

<div id="example-350" class="example">

<div class="examplenum">

[Example 350](#example-350)

</div>

<div class="column">

    ` foo `` bar `

</div>

<div class="column">

    <p><code>foo `` bar</code></p>

</div>

</div>

Code span backticks have higher precedence than any other inline
constructs except HTML tags and autolinks. Thus, for example, this is
not parsed as emphasized text, since the second `*` is part of a code
span:

<div id="example-351" class="example">

<div class="examplenum">

[Example 351](#example-351)

</div>

<div class="column">

    *foo`*`

</div>

<div class="column">

    <p>*foo<code>*</code></p>

</div>

</div>

And this is not parsed as a link:

<div id="example-352" class="example">

<div class="examplenum">

[Example 352](#example-352)

</div>

<div class="column">

    [not a `link](/foo`)

</div>

<div class="column">

    <p>[not a <code>link](/foo</code>)</p>

</div>

</div>

Code spans, HTML tags, and autolinks have the same precedence. Thus,
this is code:

<div id="example-353" class="example">

<div class="examplenum">

[Example 353](#example-353)

</div>

<div class="column">

    `<a href="`">`

</div>

<div class="column">

    <p><code>&lt;a href=&quot;</code>&quot;&gt;`</p>

</div>

</div>

But this is an HTML tag:

<div id="example-354" class="example">

<div class="examplenum">

[Example 354](#example-354)

</div>

<div class="column">

    <a href="`">`

</div>

<div class="column">

    <p><a href="`">`</p>

</div>

</div>

And this is code:

<div id="example-355" class="example">

<div class="examplenum">

[Example 355](#example-355)

</div>

<div class="column">

    `<http://foo.bar.`baz>`

</div>

<div class="column">

    <p><code>&lt;http://foo.bar.</code>baz&gt;`</p>

</div>

</div>

But this is an autolink:

<div id="example-356" class="example">

<div class="examplenum">

[Example 356](#example-356)

</div>

<div class="column">

    <http://foo.bar.`baz>`

</div>

<div class="column">

    <p><a href="http://foo.bar.%60baz">http://foo.bar.`baz</a>`</p>

</div>

</div>

When a backtick string is not closed by a matching backtick string, we
just have literal backticks:

<div id="example-357" class="example">

<div class="examplenum">

[Example 357](#example-357)

</div>

<div class="column">

    ```foo``

</div>

<div class="column">

    <p>```foo``</p>

</div>

</div>

<div id="example-358" class="example">

<div class="examplenum">

[Example 358](#example-358)

</div>

<div class="column">

    `foo

</div>

<div class="column">

    <p>`foo</p>

</div>

</div>

The following case also illustrates the need for opening and closing
backtick strings to be equal in length:

<div id="example-359" class="example">

<div class="examplenum">

[Example 359](#example-359)

</div>

<div class="column">

    `foo``bar``

</div>

<div class="column">

    <p>`foo<code>bar</code></p>

</div>

</div>

## [](#TOC)<span class="number">6.4</span>Emphasis and strong emphasis

John Gruber’s original [Markdown syntax
description](http://daringfireball.net/projects/markdown/syntax#em)
says:

> Markdown treats asterisks (`*`) and underscores (`_`) as indicators of
> emphasis. Text wrapped with one `*` or `_` will be wrapped with an
> HTML `<em>` tag; double `*`’s or `_`’s will be wrapped with an HTML
> `<strong>` tag.

This is enough for most users, but these rules leave much undecided,
especially when it comes to nested emphasis. The original `Markdown.pl`
test suite makes it clear that triple `***` and `___` delimiters can be
used for strong emphasis, and most implementations have also allowed the
following patterns:

    ***strong emph***
    ***strong** in emph*
    ***emph* in strong**
    **in strong *emph***
    *in emph **strong***

The following patterns are less widely supported, but the intent is
clear and they are useful (especially in contexts like bibliography
entries):

    *emph *with emph* in it*
    **strong **with strong** in it**

Many implementations have also restricted intraword emphasis to the `*`
forms, to avoid unwanted emphasis in words containing internal
underscores. (It is best practice to put these in code spans, but users
often do not.)

    internal emphasis: foo*bar*baz
    no emphasis: foo_bar_baz

The rules given below capture all of these patterns, while allowing for
efficient parsing strategies that do not backtrack.

First, some definitions. A [delimiter run](#delimiter-run) is either a
sequence of one or more `*` characters that is not preceded or followed
by a non-backslash-escaped `*` character, or a sequence of one or more
`_` characters that is not preceded or followed by a
non-backslash-escaped `_` character.

A [left-flanking delimiter run](#left-flanking-delimiter-run) is a
[delimiter run](#delimiter-run) that is (1) not followed by [Unicode
whitespace](#unicode-whitespace), and either (2a) not followed by a
[punctuation character](#punctuation-character), or (2b) followed by a
[punctuation character](#punctuation-character) and preceded by [Unicode
whitespace](#unicode-whitespace) or a [punctuation
character](#punctuation-character). For purposes of this definition, the
beginning and the end of the line count as Unicode whitespace.

A [right-flanking delimiter run](#right-flanking-delimiter-run) is a
[delimiter run](#delimiter-run) that is (1) not preceded by [Unicode
whitespace](#unicode-whitespace), and either (2a) not preceded by a
[punctuation character](#punctuation-character), or (2b) preceded by a
[punctuation character](#punctuation-character) and followed by [Unicode
whitespace](#unicode-whitespace) or a [punctuation
character](#punctuation-character). For purposes of this definition, the
beginning and the end of the line count as Unicode whitespace.

Here are some examples of delimiter runs.

  - left-flanking but not right-flanking:
    
        ***abc
          _abc
        **"abc"
         _"abc"

  - right-flanking but not left-flanking:
    
    ``` 
     abc***
     abc_
    "abc"**
    "abc"_
    ```

  - Both left and right-flanking:
    
    ``` 
     abc***def
    "abc"_"def"
    ```

  - Neither left nor right-flanking:
    
        abc *** def
        a _ b

(The idea of distinguishing left-flanking and right-flanking delimiter
runs based on the character before and the character after comes from
Roopesh Chander’s
[vfmd](http://www.vfmd.org/vfmd-spec/specification/#procedure-for-identifying-emphasis-tags).
vfmd uses the terminology “emphasis indicator string” instead of
“delimiter run,” and its rules for distinguishing left- and
right-flanking runs are a bit more complex than the ones given here.)

The following rules define emphasis and strong emphasis:

1.  A single `*` character [can open emphasis](#can-open-emphasis) iff
    (if and only if) it is part of a [left-flanking delimiter
    run](#left-flanking-delimiter-run).

2.  A single `_` character [can open emphasis](#can-open-emphasis) iff
    it is part of a [left-flanking delimiter
    run](#left-flanking-delimiter-run) and either (a) not part of a
    [right-flanking delimiter run](#right-flanking-delimiter-run) or (b)
    part of a [right-flanking delimiter
    run](#right-flanking-delimiter-run) preceded by punctuation.

3.  A single `*` character [can close emphasis](#can-close-emphasis) iff
    it is part of a [right-flanking delimiter
    run](#right-flanking-delimiter-run).

4.  A single `_` character [can close emphasis](#can-close-emphasis) iff
    it is part of a [right-flanking delimiter
    run](#right-flanking-delimiter-run) and either (a) not part of a
    [left-flanking delimiter run](#left-flanking-delimiter-run) or (b)
    part of a [left-flanking delimiter
    run](#left-flanking-delimiter-run) followed by punctuation.

5.  A double `**` [can open strong emphasis](#can-open-strong-emphasis)
    iff it is part of a [left-flanking delimiter
    run](#left-flanking-delimiter-run).

6.  A double `__` [can open strong emphasis](#can-open-strong-emphasis)
    iff it is part of a [left-flanking delimiter
    run](#left-flanking-delimiter-run) and either (a) not part of a
    [right-flanking delimiter run](#right-flanking-delimiter-run) or (b)
    part of a [right-flanking delimiter
    run](#right-flanking-delimiter-run) preceded by punctuation.

7.  A double `**` [can close strong
    emphasis](#can-close-strong-emphasis) iff it is part of a
    [right-flanking delimiter run](#right-flanking-delimiter-run).

8.  A double `__` [can close strong
    emphasis](#can-close-strong-emphasis) iff it is part of a
    [right-flanking delimiter run](#right-flanking-delimiter-run) and
    either (a) not part of a [left-flanking delimiter
    run](#left-flanking-delimiter-run) or (b) part of a [left-flanking
    delimiter run](#left-flanking-delimiter-run) followed by
    punctuation.

9.  Emphasis begins with a delimiter that [can open
    emphasis](#can-open-emphasis) and ends with a delimiter that [can
    close emphasis](#can-close-emphasis), and that uses the same
    character (`_` or `*`) as the opening delimiter. The opening and
    closing delimiters must belong to separate [delimiter
    runs](#delimiter-run). If one of the delimiters can both open and
    close emphasis, then the sum of the lengths of the delimiter runs
    containing the opening and closing delimiters must not be a multiple
    of 3 unless both lengths are multiples of 3.

10. Strong emphasis begins with a delimiter that [can open strong
    emphasis](#can-open-strong-emphasis) and ends with a delimiter that
    [can close strong emphasis](#can-close-strong-emphasis), and that
    uses the same character (`_` or `*`) as the opening delimiter. The
    opening and closing delimiters must belong to separate [delimiter
    runs](#delimiter-run). If one of the delimiters can both open and
    close strong emphasis, then the sum of the lengths of the delimiter
    runs containing the opening and closing delimiters must not be a
    multiple of 3 unless both lengths are multiples of 3.

11. A literal `*` character cannot occur at the beginning or end of
    `*`-delimited emphasis or `**`-delimited strong emphasis, unless it
    is backslash-escaped.

12. A literal `_` character cannot occur at the beginning or end of
    `_`-delimited emphasis or `__`-delimited strong emphasis, unless it
    is backslash-escaped.

Where rules 1–12 above are compatible with multiple parsings, the
following principles resolve ambiguity:

13. The number of nestings should be minimized. Thus, for example, an
    interpretation `<strong>...</strong>` is always preferred to
    `<em><em>...</em></em>`.

14. An interpretation `<em><strong>...</strong></em>` is always
    preferred to `<strong><em>...</em></strong>`.

15. When two potential emphasis or strong emphasis spans overlap, so
    that the second begins before the first ends and ends after the
    first ends, the first takes precedence. Thus, for example, `*foo
    _bar* baz_` is parsed as `<em>foo _bar</em> baz_` rather than `*foo
    <em>bar* baz</em>`.

16. When there are two potential emphasis or strong emphasis spans with
    the same closing delimiter, the shorter one (the one that opens
    later) takes precedence. Thus, for example, `**foo **bar baz**` is
    parsed as `**foo <strong>bar baz</strong>` rather than `<strong>foo
    **bar baz</strong>`.

17. Inline code spans, links, images, and HTML tags group more tightly
    than emphasis. So, when there is a choice between an interpretation
    that contains one of these elements and one that does not, the
    former always wins. Thus, for example, `*[foo*](bar)` is parsed as
    `*<a href="bar">foo*</a>` rather than as `<em>[foo</em>](bar)`.

These rules can be illustrated through a series of examples.

Rule 1:

<div id="example-360" class="example">

<div class="examplenum">

[Example 360](#example-360)

</div>

<div class="column">

    *foo bar*

</div>

<div class="column">

    <p><em>foo bar</em></p>

</div>

</div>

This is not emphasis, because the opening `*` is followed by whitespace,
and hence not part of a [left-flanking delimiter
run](#left-flanking-delimiter-run):

<div id="example-361" class="example">

<div class="examplenum">

[Example 361](#example-361)

</div>

<div class="column">

    a * foo bar*

</div>

<div class="column">

    <p>a * foo bar*</p>

</div>

</div>

This is not emphasis, because the opening `*` is preceded by an
alphanumeric and followed by punctuation, and hence not part of a
[left-flanking delimiter run](#left-flanking-delimiter-run):

<div id="example-362" class="example">

<div class="examplenum">

[Example 362](#example-362)

</div>

<div class="column">

    a*"foo"*

</div>

<div class="column">

    <p>a*&quot;foo&quot;*</p>

</div>

</div>

Unicode nonbreaking spaces count as whitespace, too:

<div id="example-363" class="example">

<div class="examplenum">

[Example 363](#example-363)

</div>

<div class="column">

    * a *

</div>

<div class="column">

    <p>* a *</p>

</div>

</div>

Intraword emphasis with `*` is permitted:

<div id="example-364" class="example">

<div class="examplenum">

[Example 364](#example-364)

</div>

<div class="column">

    foo*bar*

</div>

<div class="column">

    <p>foo<em>bar</em></p>

</div>

</div>

<div id="example-365" class="example">

<div class="examplenum">

[Example 365](#example-365)

</div>

<div class="column">

    5*6*78

</div>

<div class="column">

    <p>5<em>6</em>78</p>

</div>

</div>

Rule 2:

<div id="example-366" class="example">

<div class="examplenum">

[Example 366](#example-366)

</div>

<div class="column">

    _foo bar_

</div>

<div class="column">

    <p><em>foo bar</em></p>

</div>

</div>

This is not emphasis, because the opening `_` is followed by whitespace:

<div id="example-367" class="example">

<div class="examplenum">

[Example 367](#example-367)

</div>

<div class="column">

    _ foo bar_

</div>

<div class="column">

    <p>_ foo bar_</p>

</div>

</div>

This is not emphasis, because the opening `_` is preceded by an
alphanumeric and followed by punctuation:

<div id="example-368" class="example">

<div class="examplenum">

[Example 368](#example-368)

</div>

<div class="column">

    a_"foo"_

</div>

<div class="column">

    <p>a_&quot;foo&quot;_</p>

</div>

</div>

Emphasis with `_` is not allowed inside words:

<div id="example-369" class="example">

<div class="examplenum">

[Example 369](#example-369)

</div>

<div class="column">

    foo_bar_

</div>

<div class="column">

    <p>foo_bar_</p>

</div>

</div>

<div id="example-370" class="example">

<div class="examplenum">

[Example 370](#example-370)

</div>

<div class="column">

    5_6_78

</div>

<div class="column">

    <p>5_6_78</p>

</div>

</div>

<div id="example-371" class="example">

<div class="examplenum">

[Example 371](#example-371)

</div>

<div class="column">

    пристаням_стремятся_

</div>

<div class="column">

    <p>пристаням_стремятся_</p>

</div>

</div>

Here `_` does not generate emphasis, because the first delimiter run is
right-flanking and the second left-flanking:

<div id="example-372" class="example">

<div class="examplenum">

[Example 372](#example-372)

</div>

<div class="column">

    aa_"bb"_cc

</div>

<div class="column">

    <p>aa_&quot;bb&quot;_cc</p>

</div>

</div>

This is emphasis, even though the opening delimiter is both left- and
right-flanking, because it is preceded by punctuation:

<div id="example-373" class="example">

<div class="examplenum">

[Example 373](#example-373)

</div>

<div class="column">

    foo-_(bar)_

</div>

<div class="column">

    <p>foo-<em>(bar)</em></p>

</div>

</div>

Rule 3:

This is not emphasis, because the closing delimiter does not match the
opening delimiter:

<div id="example-374" class="example">

<div class="examplenum">

[Example 374](#example-374)

</div>

<div class="column">

    _foo*

</div>

<div class="column">

    <p>_foo*</p>

</div>

</div>

This is not emphasis, because the closing `*` is preceded by whitespace:

<div id="example-375" class="example">

<div class="examplenum">

[Example 375](#example-375)

</div>

<div class="column">

    *foo bar *

</div>

<div class="column">

    <p>*foo bar *</p>

</div>

</div>

A newline also counts as whitespace:

<div id="example-376" class="example">

<div class="examplenum">

[Example 376](#example-376)

</div>

<div class="column">

    *foo bar
    *

</div>

<div class="column">

    <p>*foo bar
    *</p>

</div>

</div>

This is not emphasis, because the second `*` is preceded by punctuation
and followed by an alphanumeric (hence it is not part of a
[right-flanking delimiter run](#right-flanking-delimiter-run):

<div id="example-377" class="example">

<div class="examplenum">

[Example 377](#example-377)

</div>

<div class="column">

    *(*foo)

</div>

<div class="column">

    <p>*(*foo)</p>

</div>

</div>

The point of this restriction is more easily appreciated with this
example:

<div id="example-378" class="example">

<div class="examplenum">

[Example 378](#example-378)

</div>

<div class="column">

    *(*foo*)*

</div>

<div class="column">

    <p><em>(<em>foo</em>)</em></p>

</div>

</div>

Intraword emphasis with `*` is allowed:

<div id="example-379" class="example">

<div class="examplenum">

[Example 379](#example-379)

</div>

<div class="column">

    *foo*bar

</div>

<div class="column">

    <p><em>foo</em>bar</p>

</div>

</div>

Rule 4:

This is not emphasis, because the closing `_` is preceded by whitespace:

<div id="example-380" class="example">

<div class="examplenum">

[Example 380](#example-380)

</div>

<div class="column">

    _foo bar _

</div>

<div class="column">

    <p>_foo bar _</p>

</div>

</div>

This is not emphasis, because the second `_` is preceded by punctuation
and followed by an alphanumeric:

<div id="example-381" class="example">

<div class="examplenum">

[Example 381](#example-381)

</div>

<div class="column">

    _(_foo)

</div>

<div class="column">

    <p>_(_foo)</p>

</div>

</div>

This is emphasis within emphasis:

<div id="example-382" class="example">

<div class="examplenum">

[Example 382](#example-382)

</div>

<div class="column">

    _(_foo_)_

</div>

<div class="column">

    <p><em>(<em>foo</em>)</em></p>

</div>

</div>

Intraword emphasis is disallowed for `_`:

<div id="example-383" class="example">

<div class="examplenum">

[Example 383](#example-383)

</div>

<div class="column">

    _foo_bar

</div>

<div class="column">

    <p>_foo_bar</p>

</div>

</div>

<div id="example-384" class="example">

<div class="examplenum">

[Example 384](#example-384)

</div>

<div class="column">

    _пристаням_стремятся

</div>

<div class="column">

    <p>_пристаням_стремятся</p>

</div>

</div>

<div id="example-385" class="example">

<div class="examplenum">

[Example 385](#example-385)

</div>

<div class="column">

    _foo_bar_baz_

</div>

<div class="column">

    <p><em>foo_bar_baz</em></p>

</div>

</div>

This is emphasis, even though the closing delimiter is both left- and
right-flanking, because it is followed by punctuation:

<div id="example-386" class="example">

<div class="examplenum">

[Example 386](#example-386)

</div>

<div class="column">

    _(bar)_.

</div>

<div class="column">

    <p><em>(bar)</em>.</p>

</div>

</div>

Rule 5:

<div id="example-387" class="example">

<div class="examplenum">

[Example 387](#example-387)

</div>

<div class="column">

    **foo bar**

</div>

<div class="column">

    <p><strong>foo bar</strong></p>

</div>

</div>

This is not strong emphasis, because the opening delimiter is followed
by whitespace:

<div id="example-388" class="example">

<div class="examplenum">

[Example 388](#example-388)

</div>

<div class="column">

    ** foo bar**

</div>

<div class="column">

    <p>** foo bar**</p>

</div>

</div>

This is not strong emphasis, because the opening `**` is preceded by an
alphanumeric and followed by punctuation, and hence not part of a
[left-flanking delimiter run](#left-flanking-delimiter-run):

<div id="example-389" class="example">

<div class="examplenum">

[Example 389](#example-389)

</div>

<div class="column">

    a**"foo"**

</div>

<div class="column">

    <p>a**&quot;foo&quot;**</p>

</div>

</div>

Intraword strong emphasis with `**` is permitted:

<div id="example-390" class="example">

<div class="examplenum">

[Example 390](#example-390)

</div>

<div class="column">

    foo**bar**

</div>

<div class="column">

    <p>foo<strong>bar</strong></p>

</div>

</div>

Rule 6:

<div id="example-391" class="example">

<div class="examplenum">

[Example 391](#example-391)

</div>

<div class="column">

    __foo bar__

</div>

<div class="column">

    <p><strong>foo bar</strong></p>

</div>

</div>

This is not strong emphasis, because the opening delimiter is followed
by whitespace:

<div id="example-392" class="example">

<div class="examplenum">

[Example 392](#example-392)

</div>

<div class="column">

    __ foo bar__

</div>

<div class="column">

    <p>__ foo bar__</p>

</div>

</div>

A newline counts as whitespace:

<div id="example-393" class="example">

<div class="examplenum">

[Example 393](#example-393)

</div>

<div class="column">

    __
    foo bar__

</div>

<div class="column">

    <p>__
    foo bar__</p>

</div>

</div>

This is not strong emphasis, because the opening `__` is preceded by an
alphanumeric and followed by punctuation:

<div id="example-394" class="example">

<div class="examplenum">

[Example 394](#example-394)

</div>

<div class="column">

    a__"foo"__

</div>

<div class="column">

    <p>a__&quot;foo&quot;__</p>

</div>

</div>

Intraword strong emphasis is forbidden with `__`:

<div id="example-395" class="example">

<div class="examplenum">

[Example 395](#example-395)

</div>

<div class="column">

    foo__bar__

</div>

<div class="column">

    <p>foo__bar__</p>

</div>

</div>

<div id="example-396" class="example">

<div class="examplenum">

[Example 396](#example-396)

</div>

<div class="column">

    5__6__78

</div>

<div class="column">

    <p>5__6__78</p>

</div>

</div>

<div id="example-397" class="example">

<div class="examplenum">

[Example 397](#example-397)

</div>

<div class="column">

    пристаням__стремятся__

</div>

<div class="column">

    <p>пристаням__стремятся__</p>

</div>

</div>

<div id="example-398" class="example">

<div class="examplenum">

[Example 398](#example-398)

</div>

<div class="column">

    __foo, __bar__, baz__

</div>

<div class="column">

    <p><strong>foo, <strong>bar</strong>, baz</strong></p>

</div>

</div>

This is strong emphasis, even though the opening delimiter is both left-
and right-flanking, because it is preceded by punctuation:

<div id="example-399" class="example">

<div class="examplenum">

[Example 399](#example-399)

</div>

<div class="column">

    foo-__(bar)__

</div>

<div class="column">

    <p>foo-<strong>(bar)</strong></p>

</div>

</div>

Rule 7:

This is not strong emphasis, because the closing delimiter is preceded
by whitespace:

<div id="example-400" class="example">

<div class="examplenum">

[Example 400](#example-400)

</div>

<div class="column">

    **foo bar **

</div>

<div class="column">

    <p>**foo bar **</p>

</div>

</div>

(Nor can it be interpreted as an emphasized `*foo bar *`, because of
Rule 11.)

This is not strong emphasis, because the second `**` is preceded by
punctuation and followed by an alphanumeric:

<div id="example-401" class="example">

<div class="examplenum">

[Example 401](#example-401)

</div>

<div class="column">

    **(**foo)

</div>

<div class="column">

    <p>**(**foo)</p>

</div>

</div>

The point of this restriction is more easily appreciated with these
examples:

<div id="example-402" class="example">

<div class="examplenum">

[Example 402](#example-402)

</div>

<div class="column">

    *(**foo**)*

</div>

<div class="column">

    <p><em>(<strong>foo</strong>)</em></p>

</div>

</div>

<div id="example-403" class="example">

<div class="examplenum">

[Example 403](#example-403)

</div>

<div class="column">

    **Gomphocarpus (*Gomphocarpus physocarpus*, syn.
    *Asclepias physocarpa*)**

</div>

<div class="column">

    <p><strong>Gomphocarpus (<em>Gomphocarpus physocarpus</em>, syn.
    <em>Asclepias physocarpa</em>)</strong></p>

</div>

</div>

<div id="example-404" class="example">

<div class="examplenum">

[Example 404](#example-404)

</div>

<div class="column">

    **foo "*bar*" foo**

</div>

<div class="column">

    <p><strong>foo &quot;<em>bar</em>&quot; foo</strong></p>

</div>

</div>

Intraword emphasis:

<div id="example-405" class="example">

<div class="examplenum">

[Example 405](#example-405)

</div>

<div class="column">

    **foo**bar

</div>

<div class="column">

    <p><strong>foo</strong>bar</p>

</div>

</div>

Rule 8:

This is not strong emphasis, because the closing delimiter is preceded
by whitespace:

<div id="example-406" class="example">

<div class="examplenum">

[Example 406](#example-406)

</div>

<div class="column">

    __foo bar __

</div>

<div class="column">

    <p>__foo bar __</p>

</div>

</div>

This is not strong emphasis, because the second `__` is preceded by
punctuation and followed by an alphanumeric:

<div id="example-407" class="example">

<div class="examplenum">

[Example 407](#example-407)

</div>

<div class="column">

    __(__foo)

</div>

<div class="column">

    <p>__(__foo)</p>

</div>

</div>

The point of this restriction is more easily appreciated with this
example:

<div id="example-408" class="example">

<div class="examplenum">

[Example 408](#example-408)

</div>

<div class="column">

    _(__foo__)_

</div>

<div class="column">

    <p><em>(<strong>foo</strong>)</em></p>

</div>

</div>

Intraword strong emphasis is forbidden with `__`:

<div id="example-409" class="example">

<div class="examplenum">

[Example 409](#example-409)

</div>

<div class="column">

    __foo__bar

</div>

<div class="column">

    <p>__foo__bar</p>

</div>

</div>

<div id="example-410" class="example">

<div class="examplenum">

[Example 410](#example-410)

</div>

<div class="column">

    __пристаням__стремятся

</div>

<div class="column">

    <p>__пристаням__стремятся</p>

</div>

</div>

<div id="example-411" class="example">

<div class="examplenum">

[Example 411](#example-411)

</div>

<div class="column">

    __foo__bar__baz__

</div>

<div class="column">

    <p><strong>foo__bar__baz</strong></p>

</div>

</div>

This is strong emphasis, even though the closing delimiter is both left-
and right-flanking, because it is followed by punctuation:

<div id="example-412" class="example">

<div class="examplenum">

[Example 412](#example-412)

</div>

<div class="column">

    __(bar)__.

</div>

<div class="column">

    <p><strong>(bar)</strong>.</p>

</div>

</div>

Rule 9:

Any nonempty sequence of inline elements can be the contents of an
emphasized span.

<div id="example-413" class="example">

<div class="examplenum">

[Example 413](#example-413)

</div>

<div class="column">

    *foo [bar](/url)*

</div>

<div class="column">

    <p><em>foo <a href="/url">bar</a></em></p>

</div>

</div>

<div id="example-414" class="example">

<div class="examplenum">

[Example 414](#example-414)

</div>

<div class="column">

    *foo
    bar*

</div>

<div class="column">

    <p><em>foo
    bar</em></p>

</div>

</div>

In particular, emphasis and strong emphasis can be nested inside
emphasis:

<div id="example-415" class="example">

<div class="examplenum">

[Example 415](#example-415)

</div>

<div class="column">

    _foo __bar__ baz_

</div>

<div class="column">

    <p><em>foo <strong>bar</strong> baz</em></p>

</div>

</div>

<div id="example-416" class="example">

<div class="examplenum">

[Example 416](#example-416)

</div>

<div class="column">

    _foo _bar_ baz_

</div>

<div class="column">

    <p><em>foo <em>bar</em> baz</em></p>

</div>

</div>

<div id="example-417" class="example">

<div class="examplenum">

[Example 417](#example-417)

</div>

<div class="column">

    __foo_ bar_

</div>

<div class="column">

    <p><em><em>foo</em> bar</em></p>

</div>

</div>

<div id="example-418" class="example">

<div class="examplenum">

[Example 418](#example-418)

</div>

<div class="column">

    *foo *bar**

</div>

<div class="column">

    <p><em>foo <em>bar</em></em></p>

</div>

</div>

<div id="example-419" class="example">

<div class="examplenum">

[Example 419](#example-419)

</div>

<div class="column">

    *foo **bar** baz*

</div>

<div class="column">

    <p><em>foo <strong>bar</strong> baz</em></p>

</div>

</div>

<div id="example-420" class="example">

<div class="examplenum">

[Example 420](#example-420)

</div>

<div class="column">

    *foo**bar**baz*

</div>

<div class="column">

    <p><em>foo<strong>bar</strong>baz</em></p>

</div>

</div>

Note that in the preceding case, the interpretation

    <p><em>foo</em><em>bar<em></em>baz</em></p>

is precluded by the condition that a delimiter that can both open and
close (like the `*` after `foo`) cannot form emphasis if the sum of the
lengths of the delimiter runs containing the opening and closing
delimiters is a multiple of 3 unless both lengths are multiples of 3.

For the same reason, we don’t get two consecutive emphasis sections in
this example:

<div id="example-421" class="example">

<div class="examplenum">

[Example 421](#example-421)

</div>

<div class="column">

    *foo**bar*

</div>

<div class="column">

    <p><em>foo**bar</em></p>

</div>

</div>

The same condition ensures that the following cases are all strong
emphasis nested inside emphasis, even when the interior spaces are
omitted:

<div id="example-422" class="example">

<div class="examplenum">

[Example 422](#example-422)

</div>

<div class="column">

    ***foo** bar*

</div>

<div class="column">

    <p><em><strong>foo</strong> bar</em></p>

</div>

</div>

<div id="example-423" class="example">

<div class="examplenum">

[Example 423](#example-423)

</div>

<div class="column">

    *foo **bar***

</div>

<div class="column">

    <p><em>foo <strong>bar</strong></em></p>

</div>

</div>

<div id="example-424" class="example">

<div class="examplenum">

[Example 424](#example-424)

</div>

<div class="column">

    *foo**bar***

</div>

<div class="column">

    <p><em>foo<strong>bar</strong></em></p>

</div>

</div>

When the lengths of the interior closing and opening delimiter runs are
*both* multiples of 3, though, they can match to create emphasis:

<div id="example-425" class="example">

<div class="examplenum">

[Example 425](#example-425)

</div>

<div class="column">

    foo***bar***baz

</div>

<div class="column">

    <p>foo<em><strong>bar</strong></em>baz</p>

</div>

</div>

<div id="example-426" class="example">

<div class="examplenum">

[Example 426](#example-426)

</div>

<div class="column">

    foo******bar*********baz

</div>

<div class="column">

    <p>foo<strong><strong><strong>bar</strong></strong></strong>***baz</p>

</div>

</div>

Indefinite levels of nesting are possible:

<div id="example-427" class="example">

<div class="examplenum">

[Example 427](#example-427)

</div>

<div class="column">

    *foo **bar *baz* bim** bop*

</div>

<div class="column">

    <p><em>foo <strong>bar <em>baz</em> bim</strong> bop</em></p>

</div>

</div>

<div id="example-428" class="example">

<div class="examplenum">

[Example 428](#example-428)

</div>

<div class="column">

    *foo [*bar*](/url)*

</div>

<div class="column">

    <p><em>foo <a href="/url"><em>bar</em></a></em></p>

</div>

</div>

There can be no empty emphasis or strong emphasis:

<div id="example-429" class="example">

<div class="examplenum">

[Example 429](#example-429)

</div>

<div class="column">

    ** is not an empty emphasis

</div>

<div class="column">

    <p>** is not an empty emphasis</p>

</div>

</div>

<div id="example-430" class="example">

<div class="examplenum">

[Example 430](#example-430)

</div>

<div class="column">

    **** is not an empty strong emphasis

</div>

<div class="column">

    <p>**** is not an empty strong emphasis</p>

</div>

</div>

Rule 10:

Any nonempty sequence of inline elements can be the contents of an
strongly emphasized span.

<div id="example-431" class="example">

<div class="examplenum">

[Example 431](#example-431)

</div>

<div class="column">

    **foo [bar](/url)**

</div>

<div class="column">

    <p><strong>foo <a href="/url">bar</a></strong></p>

</div>

</div>

<div id="example-432" class="example">

<div class="examplenum">

[Example 432](#example-432)

</div>

<div class="column">

    **foo
    bar**

</div>

<div class="column">

    <p><strong>foo
    bar</strong></p>

</div>

</div>

In particular, emphasis and strong emphasis can be nested inside strong
emphasis:

<div id="example-433" class="example">

<div class="examplenum">

[Example 433](#example-433)

</div>

<div class="column">

    __foo _bar_ baz__

</div>

<div class="column">

    <p><strong>foo <em>bar</em> baz</strong></p>

</div>

</div>

<div id="example-434" class="example">

<div class="examplenum">

[Example 434](#example-434)

</div>

<div class="column">

    __foo __bar__ baz__

</div>

<div class="column">

    <p><strong>foo <strong>bar</strong> baz</strong></p>

</div>

</div>

<div id="example-435" class="example">

<div class="examplenum">

[Example 435](#example-435)

</div>

<div class="column">

    ____foo__ bar__

</div>

<div class="column">

    <p><strong><strong>foo</strong> bar</strong></p>

</div>

</div>

<div id="example-436" class="example">

<div class="examplenum">

[Example 436](#example-436)

</div>

<div class="column">

    **foo **bar****

</div>

<div class="column">

    <p><strong>foo <strong>bar</strong></strong></p>

</div>

</div>

<div id="example-437" class="example">

<div class="examplenum">

[Example 437](#example-437)

</div>

<div class="column">

    **foo *bar* baz**

</div>

<div class="column">

    <p><strong>foo <em>bar</em> baz</strong></p>

</div>

</div>

<div id="example-438" class="example">

<div class="examplenum">

[Example 438](#example-438)

</div>

<div class="column">

    **foo*bar*baz**

</div>

<div class="column">

    <p><strong>foo<em>bar</em>baz</strong></p>

</div>

</div>

<div id="example-439" class="example">

<div class="examplenum">

[Example 439](#example-439)

</div>

<div class="column">

    ***foo* bar**

</div>

<div class="column">

    <p><strong><em>foo</em> bar</strong></p>

</div>

</div>

<div id="example-440" class="example">

<div class="examplenum">

[Example 440](#example-440)

</div>

<div class="column">

    **foo *bar***

</div>

<div class="column">

    <p><strong>foo <em>bar</em></strong></p>

</div>

</div>

Indefinite levels of nesting are possible:

<div id="example-441" class="example">

<div class="examplenum">

[Example 441](#example-441)

</div>

<div class="column">

    **foo *bar **baz**
    bim* bop**

</div>

<div class="column">

    <p><strong>foo <em>bar <strong>baz</strong>
    bim</em> bop</strong></p>

</div>

</div>

<div id="example-442" class="example">

<div class="examplenum">

[Example 442](#example-442)

</div>

<div class="column">

    **foo [*bar*](/url)**

</div>

<div class="column">

    <p><strong>foo <a href="/url"><em>bar</em></a></strong></p>

</div>

</div>

There can be no empty emphasis or strong emphasis:

<div id="example-443" class="example">

<div class="examplenum">

[Example 443](#example-443)

</div>

<div class="column">

    __ is not an empty emphasis

</div>

<div class="column">

    <p>__ is not an empty emphasis</p>

</div>

</div>

<div id="example-444" class="example">

<div class="examplenum">

[Example 444](#example-444)

</div>

<div class="column">

    ____ is not an empty strong emphasis

</div>

<div class="column">

    <p>____ is not an empty strong emphasis</p>

</div>

</div>

Rule 11:

<div id="example-445" class="example">

<div class="examplenum">

[Example 445](#example-445)

</div>

<div class="column">

    foo ***

</div>

<div class="column">

    <p>foo ***</p>

</div>

</div>

<div id="example-446" class="example">

<div class="examplenum">

[Example 446](#example-446)

</div>

<div class="column">

    foo *\**

</div>

<div class="column">

    <p>foo <em>*</em></p>

</div>

</div>

<div id="example-447" class="example">

<div class="examplenum">

[Example 447](#example-447)

</div>

<div class="column">

    foo *_*

</div>

<div class="column">

    <p>foo <em>_</em></p>

</div>

</div>

<div id="example-448" class="example">

<div class="examplenum">

[Example 448](#example-448)

</div>

<div class="column">

    foo *****

</div>

<div class="column">

    <p>foo *****</p>

</div>

</div>

<div id="example-449" class="example">

<div class="examplenum">

[Example 449](#example-449)

</div>

<div class="column">

    foo **\***

</div>

<div class="column">

    <p>foo <strong>*</strong></p>

</div>

</div>

<div id="example-450" class="example">

<div class="examplenum">

[Example 450](#example-450)

</div>

<div class="column">

    foo **_**

</div>

<div class="column">

    <p>foo <strong>_</strong></p>

</div>

</div>

Note that when delimiters do not match evenly, Rule 11 determines that
the excess literal `*` characters will appear outside of the emphasis,
rather than inside it:

<div id="example-451" class="example">

<div class="examplenum">

[Example 451](#example-451)

</div>

<div class="column">

    **foo*

</div>

<div class="column">

    <p>*<em>foo</em></p>

</div>

</div>

<div id="example-452" class="example">

<div class="examplenum">

[Example 452](#example-452)

</div>

<div class="column">

    *foo**

</div>

<div class="column">

    <p><em>foo</em>*</p>

</div>

</div>

<div id="example-453" class="example">

<div class="examplenum">

[Example 453](#example-453)

</div>

<div class="column">

    ***foo**

</div>

<div class="column">

    <p>*<strong>foo</strong></p>

</div>

</div>

<div id="example-454" class="example">

<div class="examplenum">

[Example 454](#example-454)

</div>

<div class="column">

    ****foo*

</div>

<div class="column">

    <p>***<em>foo</em></p>

</div>

</div>

<div id="example-455" class="example">

<div class="examplenum">

[Example 455](#example-455)

</div>

<div class="column">

    **foo***

</div>

<div class="column">

    <p><strong>foo</strong>*</p>

</div>

</div>

<div id="example-456" class="example">

<div class="examplenum">

[Example 456](#example-456)

</div>

<div class="column">

    *foo****

</div>

<div class="column">

    <p><em>foo</em>***</p>

</div>

</div>

Rule 12:

<div id="example-457" class="example">

<div class="examplenum">

[Example 457](#example-457)

</div>

<div class="column">

    foo ___

</div>

<div class="column">

    <p>foo ___</p>

</div>

</div>

<div id="example-458" class="example">

<div class="examplenum">

[Example 458](#example-458)

</div>

<div class="column">

    foo _\__

</div>

<div class="column">

    <p>foo <em>_</em></p>

</div>

</div>

<div id="example-459" class="example">

<div class="examplenum">

[Example 459](#example-459)

</div>

<div class="column">

    foo _*_

</div>

<div class="column">

    <p>foo <em>*</em></p>

</div>

</div>

<div id="example-460" class="example">

<div class="examplenum">

[Example 460](#example-460)

</div>

<div class="column">

    foo _____

</div>

<div class="column">

    <p>foo _____</p>

</div>

</div>

<div id="example-461" class="example">

<div class="examplenum">

[Example 461](#example-461)

</div>

<div class="column">

    foo __\___

</div>

<div class="column">

    <p>foo <strong>_</strong></p>

</div>

</div>

<div id="example-462" class="example">

<div class="examplenum">

[Example 462](#example-462)

</div>

<div class="column">

    foo __*__

</div>

<div class="column">

    <p>foo <strong>*</strong></p>

</div>

</div>

<div id="example-463" class="example">

<div class="examplenum">

[Example 463](#example-463)

</div>

<div class="column">

    __foo_

</div>

<div class="column">

    <p>_<em>foo</em></p>

</div>

</div>

Note that when delimiters do not match evenly, Rule 12 determines that
the excess literal `_` characters will appear outside of the emphasis,
rather than inside it:

<div id="example-464" class="example">

<div class="examplenum">

[Example 464](#example-464)

</div>

<div class="column">

    _foo__

</div>

<div class="column">

    <p><em>foo</em>_</p>

</div>

</div>

<div id="example-465" class="example">

<div class="examplenum">

[Example 465](#example-465)

</div>

<div class="column">

    ___foo__

</div>

<div class="column">

    <p>_<strong>foo</strong></p>

</div>

</div>

<div id="example-466" class="example">

<div class="examplenum">

[Example 466](#example-466)

</div>

<div class="column">

    ____foo_

</div>

<div class="column">

    <p>___<em>foo</em></p>

</div>

</div>

<div id="example-467" class="example">

<div class="examplenum">

[Example 467](#example-467)

</div>

<div class="column">

    __foo___

</div>

<div class="column">

    <p><strong>foo</strong>_</p>

</div>

</div>

<div id="example-468" class="example">

<div class="examplenum">

[Example 468](#example-468)

</div>

<div class="column">

    _foo____

</div>

<div class="column">

    <p><em>foo</em>___</p>

</div>

</div>

Rule 13 implies that if you want emphasis nested directly inside
emphasis, you must use different delimiters:

<div id="example-469" class="example">

<div class="examplenum">

[Example 469](#example-469)

</div>

<div class="column">

    **foo**

</div>

<div class="column">

    <p><strong>foo</strong></p>

</div>

</div>

<div id="example-470" class="example">

<div class="examplenum">

[Example 470](#example-470)

</div>

<div class="column">

    *_foo_*

</div>

<div class="column">

    <p><em><em>foo</em></em></p>

</div>

</div>

<div id="example-471" class="example">

<div class="examplenum">

[Example 471](#example-471)

</div>

<div class="column">

    __foo__

</div>

<div class="column">

    <p><strong>foo</strong></p>

</div>

</div>

<div id="example-472" class="example">

<div class="examplenum">

[Example 472](#example-472)

</div>

<div class="column">

    _*foo*_

</div>

<div class="column">

    <p><em><em>foo</em></em></p>

</div>

</div>

However, strong emphasis within strong emphasis is possible without
switching delimiters:

<div id="example-473" class="example">

<div class="examplenum">

[Example 473](#example-473)

</div>

<div class="column">

    ****foo****

</div>

<div class="column">

    <p><strong><strong>foo</strong></strong></p>

</div>

</div>

<div id="example-474" class="example">

<div class="examplenum">

[Example 474](#example-474)

</div>

<div class="column">

    ____foo____

</div>

<div class="column">

    <p><strong><strong>foo</strong></strong></p>

</div>

</div>

Rule 13 can be applied to arbitrarily long sequences of delimiters:

<div id="example-475" class="example">

<div class="examplenum">

[Example 475](#example-475)

</div>

<div class="column">

    ******foo******

</div>

<div class="column">

    <p><strong><strong><strong>foo</strong></strong></strong></p>

</div>

</div>

Rule 14:

<div id="example-476" class="example">

<div class="examplenum">

[Example 476](#example-476)

</div>

<div class="column">

    ***foo***

</div>

<div class="column">

    <p><em><strong>foo</strong></em></p>

</div>

</div>

<div id="example-477" class="example">

<div class="examplenum">

[Example 477](#example-477)

</div>

<div class="column">

    _____foo_____

</div>

<div class="column">

    <p><em><strong><strong>foo</strong></strong></em></p>

</div>

</div>

Rule 15:

<div id="example-478" class="example">

<div class="examplenum">

[Example 478](#example-478)

</div>

<div class="column">

    *foo _bar* baz_

</div>

<div class="column">

    <p><em>foo _bar</em> baz_</p>

</div>

</div>

<div id="example-479" class="example">

<div class="examplenum">

[Example 479](#example-479)

</div>

<div class="column">

    *foo __bar *baz bim__ bam*

</div>

<div class="column">

    <p><em>foo <strong>bar *baz bim</strong> bam</em></p>

</div>

</div>

Rule 16:

<div id="example-480" class="example">

<div class="examplenum">

[Example 480](#example-480)

</div>

<div class="column">

    **foo **bar baz**

</div>

<div class="column">

    <p>**foo <strong>bar baz</strong></p>

</div>

</div>

<div id="example-481" class="example">

<div class="examplenum">

[Example 481](#example-481)

</div>

<div class="column">

    *foo *bar baz*

</div>

<div class="column">

    <p>*foo <em>bar baz</em></p>

</div>

</div>

Rule 17:

<div id="example-482" class="example">

<div class="examplenum">

[Example 482](#example-482)

</div>

<div class="column">

    *[bar*](/url)

</div>

<div class="column">

    <p>*<a href="/url">bar*</a></p>

</div>

</div>

<div id="example-483" class="example">

<div class="examplenum">

[Example 483](#example-483)

</div>

<div class="column">

    _foo [bar_](/url)

</div>

<div class="column">

    <p>_foo <a href="/url">bar_</a></p>

</div>

</div>

<div id="example-484" class="example">

<div class="examplenum">

[Example 484](#example-484)

</div>

<div class="column">

    *<img src="foo" title="*"/>

</div>

<div class="column">

    <p>*<img src="foo" title="*"/></p>

</div>

</div>

<div id="example-485" class="example">

<div class="examplenum">

[Example 485](#example-485)

</div>

<div class="column">

    **<a href="**">

</div>

<div class="column">

    <p>**<a href="**"></p>

</div>

</div>

<div id="example-486" class="example">

<div class="examplenum">

[Example 486](#example-486)

</div>

<div class="column">

    __<a href="__">

</div>

<div class="column">

    <p>__<a href="__"></p>

</div>

</div>

<div id="example-487" class="example">

<div class="examplenum">

[Example 487](#example-487)

</div>

<div class="column">

    *a `*`*

</div>

<div class="column">

    <p><em>a <code>*</code></em></p>

</div>

</div>

<div id="example-488" class="example">

<div class="examplenum">

[Example 488](#example-488)

</div>

<div class="column">

    _a `_`_

</div>

<div class="column">

    <p><em>a <code>_</code></em></p>

</div>

</div>

<div id="example-489" class="example">

<div class="examplenum">

[Example 489](#example-489)

</div>

<div class="column">

    **a<http://foo.bar/?q=**>

</div>

<div class="column">

    <p>**a<a href="http://foo.bar/?q=**">http://foo.bar/?q=**</a></p>

</div>

</div>

<div id="example-490" class="example">

<div class="examplenum">

[Example 490](#example-490)

</div>

<div class="column">

    __a<http://foo.bar/?q=__>

</div>

<div class="column">

    <p>__a<a href="http://foo.bar/?q=__">http://foo.bar/?q=__</a></p>

</div>

</div>

<div class="extension">

## [](#TOC)<span class="number">6.5</span>Strikethrough (extension)

GFM enables the `strikethrough` extension, where an additional emphasis
type is available.

Strikethrough text is any text wrapped in two tildes (`~`).

<div id="example-491" class="example">

<div class="examplenum">

[Example 491](#example-491)

</div>

<div class="column">

    ~~Hi~~ Hello, world!

</div>

<div class="column">

    <p><del>Hi</del> Hello, world!</p>

</div>

</div>

As with regular emphasis delimiters, a new paragraph will cause
strikethrough parsing to cease:

<div id="example-492" class="example">

<div class="examplenum">

[Example 492](#example-492)

</div>

<div class="column">

    This ~~has a
    
    new paragraph~~.

</div>

<div class="column">

    <p>This ~~has a</p>
    <p>new paragraph~~.</p>

</div>

</div>

</div>

## [](#TOC)<span class="number">6.6</span>Links

A link contains [link text](#link-text) (the visible text), a [link
destination](#link-destination) (the URI that is the link destination),
and optionally a [link title](#link-title). There are two basic kinds of
links in Markdown. In [inline links](#inline-link) the destination and
title are given immediately after the link text. In [reference
links](#reference-link) the destination and title are defined elsewhere
in the document.

A [link text](#link-text) consists of a sequence of zero or more inline
elements enclosed by square brackets (`[` and `]`). The following rules
apply:

  - Links may not contain other links, at any level of nesting. If
    multiple otherwise valid link definitions appear nested inside each
    other, the inner-most definition is used.

  - Brackets are allowed in the [link text](#link-text) only if (a) they
    are backslash-escaped or (b) they appear as a matched pair of
    brackets, with an open bracket `[`, a sequence of zero or more
    inlines, and a close bracket `]`.

  - Backtick [code spans](#code-span), [autolinks](#autolinks), and raw
    [HTML tags](#html-tag) bind more tightly than the brackets in link
    text. Thus, for example, `` [foo`]` `` could not be a link text,
    since the second `]` is part of a code span.

  - The brackets in link text bind more tightly than markers for
    [emphasis and strong emphasis](#emphasis-and-strong-emphasis). Thus,
    for example, `*[foo*](url)` is a link.

A [link destination](#link-destination) consists of either

  - a sequence of zero or more characters between an opening `<` and a
    closing `>` that contains no line breaks or unescaped `<` or `>`
    characters, or

  - a nonempty sequence of characters that does not start with `<`, does
    not include ASCII space or control characters, and includes
    parentheses only if (a) they are backslash-escaped or (b) they are
    part of a balanced pair of unescaped parentheses. (Implementations
    may impose limits on parentheses nesting to avoid performance
    issues, but at least three levels of nesting should be supported.)

A [link title](#link-title) consists of either

  - a sequence of zero or more characters between straight double-quote
    characters (`"`), including a `"` character only if it is
    backslash-escaped, or

  - a sequence of zero or more characters between straight single-quote
    characters (`'`), including a `'` character only if it is
    backslash-escaped, or

  - a sequence of zero or more characters between matching parentheses
    (`(...)`), including a `(` or `)` character only if it is
    backslash-escaped.

Although [link titles](#link-title) may span multiple lines, they may
not contain a [blank line](#blank-line).

An [inline link](#inline-link) consists of a [link text](#link-text)
followed immediately by a left parenthesis `(`, optional
[whitespace](#whitespace), an optional [link
destination](#link-destination), an optional [link title](#link-title)
separated from the link destination by [whitespace](#whitespace),
optional [whitespace](#whitespace), and a right parenthesis `)`. The
link’s text consists of the inlines contained in the [link
text](#link-text) (excluding the enclosing square brackets). The link’s
URI consists of the link destination, excluding enclosing `<...>` if
present, with backslash-escapes in effect as described above. The link’s
title consists of the link title, excluding its enclosing delimiters,
with backslash-escapes in effect as described above.

Here is a simple inline link:

<div id="example-493" class="example">

<div class="examplenum">

[Example 493](#example-493)

</div>

<div class="column">

    [link](/uri "title")

</div>

<div class="column">

    <p><a href="/uri" title="title">link</a></p>

</div>

</div>

The title may be omitted:

<div id="example-494" class="example">

<div class="examplenum">

[Example 494](#example-494)

</div>

<div class="column">

    [link](/uri)

</div>

<div class="column">

    <p><a href="/uri">link</a></p>

</div>

</div>

Both the title and the destination may be omitted:

<div id="example-495" class="example">

<div class="examplenum">

[Example 495](#example-495)

</div>

<div class="column">

    [link]()

</div>

<div class="column">

    <p><a href="">link</a></p>

</div>

</div>

<div id="example-496" class="example">

<div class="examplenum">

[Example 496](#example-496)

</div>

<div class="column">

    [link](<>)

</div>

<div class="column">

    <p><a href="">link</a></p>

</div>

</div>

The destination can only contain spaces if it is enclosed in pointy
brackets:

<div id="example-497" class="example">

<div class="examplenum">

[Example 497](#example-497)

</div>

<div class="column">

    [link](/my uri)

</div>

<div class="column">

    <p>[link](/my uri)</p>

</div>

</div>

<div id="example-498" class="example">

<div class="examplenum">

[Example 498](#example-498)

</div>

<div class="column">

    [link](</my uri>)

</div>

<div class="column">

    <p><a href="/my%20uri">link</a></p>

</div>

</div>

The destination cannot contain line breaks, even if enclosed in pointy
brackets:

<div id="example-499" class="example">

<div class="examplenum">

[Example 499](#example-499)

</div>

<div class="column">

    [link](foo
    bar)

</div>

<div class="column">

    <p>[link](foo
    bar)</p>

</div>

</div>

<div id="example-500" class="example">

<div class="examplenum">

[Example 500](#example-500)

</div>

<div class="column">

    [link](<foo
    bar>)

</div>

<div class="column">

    <p>[link](<foo
    bar>)</p>

</div>

</div>

The destination can contain `)` if it is enclosed in pointy brackets:

<div id="example-501" class="example">

<div class="examplenum">

[Example 501](#example-501)

</div>

<div class="column">

    [a](<b)c>)

</div>

<div class="column">

    <p><a href="b)c">a</a></p>

</div>

</div>

Pointy brackets that enclose links must be unescaped:

<div id="example-502" class="example">

<div class="examplenum">

[Example 502](#example-502)

</div>

<div class="column">

    [link](<foo\>)

</div>

<div class="column">

    <p>[link](&lt;foo&gt;)</p>

</div>

</div>

These are not links, because the opening pointy bracket is not matched
properly:

<div id="example-503" class="example">

<div class="examplenum">

[Example 503](#example-503)

</div>

<div class="column">

    [a](<b)c
    [a](<b)c>
    [a](<b>c)

</div>

<div class="column">

    <p>[a](&lt;b)c
    [a](&lt;b)c&gt;
    [a](<b>c)</p>

</div>

</div>

Parentheses inside the link destination may be escaped:

<div id="example-504" class="example">

<div class="examplenum">

[Example 504](#example-504)

</div>

<div class="column">

    [link](\(foo\))

</div>

<div class="column">

    <p><a href="(foo)">link</a></p>

</div>

</div>

Any number of parentheses are allowed without escaping, as long as they
are balanced:

<div id="example-505" class="example">

<div class="examplenum">

[Example 505](#example-505)

</div>

<div class="column">

    [link](foo(and(bar)))

</div>

<div class="column">

    <p><a href="foo(and(bar))">link</a></p>

</div>

</div>

However, if you have unbalanced parentheses, you need to escape or use
the `<...>` form:

<div id="example-506" class="example">

<div class="examplenum">

[Example 506](#example-506)

</div>

<div class="column">

    [link](foo\(and\(bar\))

</div>

<div class="column">

    <p><a href="foo(and(bar)">link</a></p>

</div>

</div>

<div id="example-507" class="example">

<div class="examplenum">

[Example 507](#example-507)

</div>

<div class="column">

    [link](<foo(and(bar)>)

</div>

<div class="column">

    <p><a href="foo(and(bar)">link</a></p>

</div>

</div>

Parentheses and other symbols can also be escaped, as usual in Markdown:

<div id="example-508" class="example">

<div class="examplenum">

[Example 508](#example-508)

</div>

<div class="column">

    [link](foo\)\:)

</div>

<div class="column">

    <p><a href="foo):">link</a></p>

</div>

</div>

A link can contain fragment identifiers and queries:

<div id="example-509" class="example">

<div class="examplenum">

[Example 509](#example-509)

</div>

<div class="column">

    [link](#fragment)
    
    [link](http://example.com#fragment)
    
    [link](http://example.com?foo=3#frag)

</div>

<div class="column">

    <p><a href="#fragment">link</a></p>
    <p><a href="http://example.com#fragment">link</a></p>
    <p><a href="http://example.com?foo=3#frag">link</a></p>

</div>

</div>

Note that a backslash before a non-escapable character is just a
backslash:

<div id="example-510" class="example">

<div class="examplenum">

[Example 510](#example-510)

</div>

<div class="column">

    [link](foo\bar)

</div>

<div class="column">

    <p><a href="foo%5Cbar">link</a></p>

</div>

</div>

URL-escaping should be left alone inside the destination, as all
URL-escaped characters are also valid URL characters. Entity and
numerical character references in the destination will be parsed into
the corresponding Unicode code points, as usual. These may be optionally
URL-escaped when written as HTML, but this spec does not enforce any
particular policy for rendering URLs in HTML or other formats. Renderers
may make different decisions about how to escape or normalize URLs in
the output.

<div id="example-511" class="example">

<div class="examplenum">

[Example 511](#example-511)

</div>

<div class="column">

    [link](foo%20b&auml;)

</div>

<div class="column">

    <p><a href="foo%20b%C3%A4">link</a></p>

</div>

</div>

Note that, because titles can often be parsed as destinations, if you
try to omit the destination and keep the title, you’ll get unexpected
results:

<div id="example-512" class="example">

<div class="examplenum">

[Example 512](#example-512)

</div>

<div class="column">

    [link]("title")

</div>

<div class="column">

    <p><a href="%22title%22">link</a></p>

</div>

</div>

Titles may be in single quotes, double quotes, or parentheses:

<div id="example-513" class="example">

<div class="examplenum">

[Example 513](#example-513)

</div>

<div class="column">

    [link](/url "title")
    [link](/url 'title')
    [link](/url (title))

</div>

<div class="column">

    <p><a href="/url" title="title">link</a>
    <a href="/url" title="title">link</a>
    <a href="/url" title="title">link</a></p>

</div>

</div>

Backslash escapes and entity and numeric character references may be
used in titles:

<div id="example-514" class="example">

<div class="examplenum">

[Example 514](#example-514)

</div>

<div class="column">

    [link](/url "title \"&quot;")

</div>

<div class="column">

    <p><a href="/url" title="title &quot;&quot;">link</a></p>

</div>

</div>

Titles must be separated from the link using a
[whitespace](#whitespace). Other [Unicode
whitespace](#unicode-whitespace) like non-breaking space doesn’t work.

<div id="example-515" class="example">

<div class="examplenum">

[Example 515](#example-515)

</div>

<div class="column">

    [link](/url "title")

</div>

<div class="column">

    <p><a href="/url%C2%A0%22title%22">link</a></p>

</div>

</div>

Nested balanced quotes are not allowed without escaping:

<div id="example-516" class="example">

<div class="examplenum">

[Example 516](#example-516)

</div>

<div class="column">

    [link](/url "title "and" title")

</div>

<div class="column">

    <p>[link](/url &quot;title &quot;and&quot; title&quot;)</p>

</div>

</div>

But it is easy to work around this by using a different quote type:

<div id="example-517" class="example">

<div class="examplenum">

[Example 517](#example-517)

</div>

<div class="column">

    [link](/url 'title "and" title')

</div>

<div class="column">

    <p><a href="/url" title="title &quot;and&quot; title">link</a></p>

</div>

</div>

(Note: `Markdown.pl` did allow double quotes inside a double-quoted
title, and its test suite included a test demonstrating this. But it is
hard to see a good rationale for the extra complexity this brings, since
there are already many ways—backslash escaping, entity and numeric
character references, or using a different quote type for the enclosing
title—to write titles containing double quotes. `Markdown.pl`’s handling
of titles has a number of other strange features. For example, it allows
single-quoted titles in inline links, but not reference links. And, in
reference links but not inline links, it allows a title to begin with
`"` and end with `)`. `Markdown.pl` 1.0.1 even allows titles with no
closing quotation mark, though 1.0.2b8 does not. It seems preferable to
adopt a simple, rational rule that works the same way in inline links
and link reference definitions.)

[Whitespace](#whitespace) is allowed around the destination and title:

<div id="example-518" class="example">

<div class="examplenum">

[Example 518](#example-518)

</div>

<div class="column">

    [link](   /uri
      "title"  )

</div>

<div class="column">

    <p><a href="/uri" title="title">link</a></p>

</div>

</div>

But it is not allowed between the link text and the following
parenthesis:

<div id="example-519" class="example">

<div class="examplenum">

[Example 519](#example-519)

</div>

<div class="column">

    [link] (/uri)

</div>

<div class="column">

    <p>[link] (/uri)</p>

</div>

</div>

The link text may contain balanced brackets, but not unbalanced ones,
unless they are escaped:

<div id="example-520" class="example">

<div class="examplenum">

[Example 520](#example-520)

</div>

<div class="column">

    [link [foo [bar]]](/uri)

</div>

<div class="column">

    <p><a href="/uri">link [foo [bar]]</a></p>

</div>

</div>

<div id="example-521" class="example">

<div class="examplenum">

[Example 521](#example-521)

</div>

<div class="column">

    [link] bar](/uri)

</div>

<div class="column">

    <p>[link] bar](/uri)</p>

</div>

</div>

<div id="example-522" class="example">

<div class="examplenum">

[Example 522](#example-522)

</div>

<div class="column">

    [link [bar](/uri)

</div>

<div class="column">

    <p>[link <a href="/uri">bar</a></p>

</div>

</div>

<div id="example-523" class="example">

<div class="examplenum">

[Example 523](#example-523)

</div>

<div class="column">

    [link \[bar](/uri)

</div>

<div class="column">

    <p><a href="/uri">link [bar</a></p>

</div>

</div>

The link text may contain inline content:

<div id="example-524" class="example">

<div class="examplenum">

[Example 524](#example-524)

</div>

<div class="column">

    [link *foo **bar** `#`*](/uri)

</div>

<div class="column">

    <p><a href="/uri">link <em>foo <strong>bar</strong> <code>#</code></em></a></p>

</div>

</div>

<div id="example-525" class="example">

<div class="examplenum">

[Example 525](#example-525)

</div>

<div class="column">

    [![moon](moon.jpg)](/uri)

</div>

<div class="column">

    <p><a href="/uri"><img src="moon.jpg" alt="moon" /></a></p>

</div>

</div>

However, links may not contain other links, at any level of nesting.

<div id="example-526" class="example">

<div class="examplenum">

[Example 526](#example-526)

</div>

<div class="column">

    [foo [bar](/uri)](/uri)

</div>

<div class="column">

    <p>[foo <a href="/uri">bar</a>](/uri)</p>

</div>

</div>

<div id="example-527" class="example">

<div class="examplenum">

[Example 527](#example-527)

</div>

<div class="column">

    [foo *[bar [baz](/uri)](/uri)*](/uri)

</div>

<div class="column">

    <p>[foo <em>[bar <a href="/uri">baz</a>](/uri)</em>](/uri)</p>

</div>

</div>

<div id="example-528" class="example">

<div class="examplenum">

[Example 528](#example-528)

</div>

<div class="column">

    ![[[foo](uri1)](uri2)](uri3)

</div>

<div class="column">

    <p><img src="uri3" alt="[foo](uri2)" /></p>

</div>

</div>

These cases illustrate the precedence of link text grouping over
emphasis grouping:

<div id="example-529" class="example">

<div class="examplenum">

[Example 529](#example-529)

</div>

<div class="column">

    *[foo*](/uri)

</div>

<div class="column">

    <p>*<a href="/uri">foo*</a></p>

</div>

</div>

<div id="example-530" class="example">

<div class="examplenum">

[Example 530](#example-530)

</div>

<div class="column">

    [foo *bar](baz*)

</div>

<div class="column">

    <p><a href="baz*">foo *bar</a></p>

</div>

</div>

Note that brackets that *aren’t* part of links do not take precedence:

<div id="example-531" class="example">

<div class="examplenum">

[Example 531](#example-531)

</div>

<div class="column">

    *foo [bar* baz]

</div>

<div class="column">

    <p><em>foo [bar</em> baz]</p>

</div>

</div>

These cases illustrate the precedence of HTML tags, code spans, and
autolinks over link grouping:

<div id="example-532" class="example">

<div class="examplenum">

[Example 532](#example-532)

</div>

<div class="column">

    [foo <bar attr="](baz)">

</div>

<div class="column">

    <p>[foo <bar attr="](baz)"></p>

</div>

</div>

<div id="example-533" class="example">

<div class="examplenum">

[Example 533](#example-533)

</div>

<div class="column">

    [foo`](/uri)`

</div>

<div class="column">

    <p>[foo<code>](/uri)</code></p>

</div>

</div>

<div id="example-534" class="example">

<div class="examplenum">

[Example 534](#example-534)

</div>

<div class="column">

    [foo<http://example.com/?search=](uri)>

</div>

<div class="column">

    <p>[foo<a href="http://example.com/?search=%5D(uri)">http://example.com/?search=](uri)</a></p>

</div>

</div>

There are three kinds of [reference link](#reference-link)s:
[full](#full-reference-link), [collapsed](#collapsed-reference-link),
and [shortcut](#shortcut-reference-link).

A [full reference link](#full-reference-link) consists of a [link
text](#link-text) immediately followed by a [link label](#link-label)
that [matches](#matches) a [link reference
definition](#link-reference-definition) elsewhere in the document.

A [link label](#link-label) begins with a left bracket (`[`) and ends
with the first right bracket (`]`) that is not backslash-escaped.
Between these brackets there must be at least one [non-whitespace
character](#non-whitespace-character). Unescaped square bracket
characters are not allowed inside the opening and closing square
brackets of [link labels](#link-label). A link label can have at most
999 characters inside the square brackets.

One label [matches](#matches) another just in case their normalized
forms are equal. To normalize a label, strip off the opening and closing
brackets, perform the *Unicode case fold*, strip leading and trailing
[whitespace](#whitespace) and collapse consecutive internal
[whitespace](#whitespace) to a single space. If there are multiple
matching reference link definitions, the one that comes first in the
document is used. (It is desirable in such cases to emit a warning.)

The link’s URI and title are provided by the matching [link reference
definition](#link-reference-definition).

Here is a simple example:

<div id="example-535" class="example">

<div class="examplenum">

[Example 535](#example-535)

</div>

<div class="column">

    [foo][bar]
    
    [bar]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title">foo</a></p>

</div>

</div>

The rules for the [link text](#link-text) are the same as with [inline
links](#inline-link). Thus:

The link text may contain balanced brackets, but not unbalanced ones,
unless they are escaped:

<div id="example-536" class="example">

<div class="examplenum">

[Example 536](#example-536)

</div>

<div class="column">

    [link [foo [bar]]][ref]
    
    [ref]: /uri

</div>

<div class="column">

    <p><a href="/uri">link [foo [bar]]</a></p>

</div>

</div>

<div id="example-537" class="example">

<div class="examplenum">

[Example 537](#example-537)

</div>

<div class="column">

    [link \[bar][ref]
    
    [ref]: /uri

</div>

<div class="column">

    <p><a href="/uri">link [bar</a></p>

</div>

</div>

The link text may contain inline content:

<div id="example-538" class="example">

<div class="examplenum">

[Example 538](#example-538)

</div>

<div class="column">

    [link *foo **bar** `#`*][ref]
    
    [ref]: /uri

</div>

<div class="column">

    <p><a href="/uri">link <em>foo <strong>bar</strong> <code>#</code></em></a></p>

</div>

</div>

<div id="example-539" class="example">

<div class="examplenum">

[Example 539](#example-539)

</div>

<div class="column">

    [![moon](moon.jpg)][ref]
    
    [ref]: /uri

</div>

<div class="column">

    <p><a href="/uri"><img src="moon.jpg" alt="moon" /></a></p>

</div>

</div>

However, links may not contain other links, at any level of nesting.

<div id="example-540" class="example">

<div class="examplenum">

[Example 540](#example-540)

</div>

<div class="column">

    [foo [bar](/uri)][ref]
    
    [ref]: /uri

</div>

<div class="column">

    <p>[foo <a href="/uri">bar</a>]<a href="/uri">ref</a></p>

</div>

</div>

<div id="example-541" class="example">

<div class="examplenum">

[Example 541](#example-541)

</div>

<div class="column">

    [foo *bar [baz][ref]*][ref]
    
    [ref]: /uri

</div>

<div class="column">

    <p>[foo <em>bar <a href="/uri">baz</a></em>]<a href="/uri">ref</a></p>

</div>

</div>

(In the examples above, we have two [shortcut reference
links](#shortcut-reference-link) instead of one [full reference
link](#full-reference-link).)

The following cases illustrate the precedence of link text grouping over
emphasis grouping:

<div id="example-542" class="example">

<div class="examplenum">

[Example 542](#example-542)

</div>

<div class="column">

    *[foo*][ref]
    
    [ref]: /uri

</div>

<div class="column">

    <p>*<a href="/uri">foo*</a></p>

</div>

</div>

<div id="example-543" class="example">

<div class="examplenum">

[Example 543](#example-543)

</div>

<div class="column">

    [foo *bar][ref]*
    
    [ref]: /uri

</div>

<div class="column">

    <p><a href="/uri">foo *bar</a>*</p>

</div>

</div>

These cases illustrate the precedence of HTML tags, code spans, and
autolinks over link grouping:

<div id="example-544" class="example">

<div class="examplenum">

[Example 544](#example-544)

</div>

<div class="column">

    [foo <bar attr="][ref]">
    
    [ref]: /uri

</div>

<div class="column">

    <p>[foo <bar attr="][ref]"></p>

</div>

</div>

<div id="example-545" class="example">

<div class="examplenum">

[Example 545](#example-545)

</div>

<div class="column">

    [foo`][ref]`
    
    [ref]: /uri

</div>

<div class="column">

    <p>[foo<code>][ref]</code></p>

</div>

</div>

<div id="example-546" class="example">

<div class="examplenum">

[Example 546](#example-546)

</div>

<div class="column">

    [foo<http://example.com/?search=][ref]>
    
    [ref]: /uri

</div>

<div class="column">

    <p>[foo<a href="http://example.com/?search=%5D%5Bref%5D">http://example.com/?search=][ref]</a></p>

</div>

</div>

Matching is case-insensitive:

<div id="example-547" class="example">

<div class="examplenum">

[Example 547](#example-547)

</div>

<div class="column">

    [foo][BaR]
    
    [bar]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title">foo</a></p>

</div>

</div>

Unicode case fold is used:

<div id="example-548" class="example">

<div class="examplenum">

[Example 548](#example-548)

</div>

<div class="column">

    [ẞ]
    
    [SS]: /url

</div>

<div class="column">

    <p><a href="/url">ẞ</a></p>

</div>

</div>

Consecutive internal [whitespace](#whitespace) is treated as one space
for purposes of determining matching:

<div id="example-549" class="example">

<div class="examplenum">

[Example 549](#example-549)

</div>

<div class="column">

    [Foo
      bar]: /url
    
    [Baz][Foo bar]

</div>

<div class="column">

    <p><a href="/url">Baz</a></p>

</div>

</div>

No [whitespace](#whitespace) is allowed between the [link
text](#link-text) and the [link label](#link-label):

<div id="example-550" class="example">

<div class="examplenum">

[Example 550](#example-550)

</div>

<div class="column">

    [foo] [bar]
    
    [bar]: /url "title"

</div>

<div class="column">

    <p>[foo] <a href="/url" title="title">bar</a></p>

</div>

</div>

<div id="example-551" class="example">

<div class="examplenum">

[Example 551](#example-551)

</div>

<div class="column">

    [foo]
    [bar]
    
    [bar]: /url "title"

</div>

<div class="column">

    <p>[foo]
    <a href="/url" title="title">bar</a></p>

</div>

</div>

This is a departure from John Gruber’s original Markdown syntax
description, which explicitly allows whitespace between the link text
and the link label. It brings reference links in line with [inline
links](#inline-link), which (according to both original Markdown and
this spec) cannot have whitespace after the link text. More importantly,
it prevents inadvertent capture of consecutive [shortcut reference
links](#shortcut-reference-link). If whitespace is allowed between the
link text and the link label, then in the following we will have a
single reference link, not two shortcut reference links, as intended:

    [foo]
    [bar]
    
    [foo]: /url1
    [bar]: /url2

(Note that [shortcut reference links](#shortcut-reference-link) were
introduced by Gruber himself in a beta version of `Markdown.pl`, but
never included in the official syntax description. Without shortcut
reference links, it is harmless to allow space between the link text and
link label; but once shortcut references are introduced, it is too
dangerous to allow this, as it frequently leads to unintended results.)

When there are multiple matching [link reference
definitions](#link-reference-definitions), the first is used:

<div id="example-552" class="example">

<div class="examplenum">

[Example 552](#example-552)

</div>

<div class="column">

    [foo]: /url1
    
    [foo]: /url2
    
    [bar][foo]

</div>

<div class="column">

    <p><a href="/url1">bar</a></p>

</div>

</div>

Note that matching is performed on normalized strings, not parsed inline
content. So the following does not match, even though the labels define
equivalent inline content:

<div id="example-553" class="example">

<div class="examplenum">

[Example 553](#example-553)

</div>

<div class="column">

    [bar][foo\!]
    
    [foo!]: /url

</div>

<div class="column">

    <p>[bar][foo!]</p>

</div>

</div>

[Link labels](#link-label) cannot contain brackets, unless they are
backslash-escaped:

<div id="example-554" class="example">

<div class="examplenum">

[Example 554](#example-554)

</div>

<div class="column">

    [foo][ref[]
    
    [ref[]: /uri

</div>

<div class="column">

    <p>[foo][ref[]</p>
    <p>[ref[]: /uri</p>

</div>

</div>

<div id="example-555" class="example">

<div class="examplenum">

[Example 555](#example-555)

</div>

<div class="column">

    [foo][ref[bar]]
    
    [ref[bar]]: /uri

</div>

<div class="column">

    <p>[foo][ref[bar]]</p>
    <p>[ref[bar]]: /uri</p>

</div>

</div>

<div id="example-556" class="example">

<div class="examplenum">

[Example 556](#example-556)

</div>

<div class="column">

    [[[foo]]]
    
    [[[foo]]]: /url

</div>

<div class="column">

    <p>[[[foo]]]</p>
    <p>[[[foo]]]: /url</p>

</div>

</div>

<div id="example-557" class="example">

<div class="examplenum">

[Example 557](#example-557)

</div>

<div class="column">

    [foo][ref\[]
    
    [ref\[]: /uri

</div>

<div class="column">

    <p><a href="/uri">foo</a></p>

</div>

</div>

Note that in this example `]` is not backslash-escaped:

<div id="example-558" class="example">

<div class="examplenum">

[Example 558](#example-558)

</div>

<div class="column">

    [bar\\]: /uri
    
    [bar\\]

</div>

<div class="column">

    <p><a href="/uri">bar\</a></p>

</div>

</div>

A [link label](#link-label) must contain at least one [non-whitespace
character](#non-whitespace-character):

<div id="example-559" class="example">

<div class="examplenum">

[Example 559](#example-559)

</div>

<div class="column">

    []
    
    []: /uri

</div>

<div class="column">

    <p>[]</p>
    <p>[]: /uri</p>

</div>

</div>

<div id="example-560" class="example">

<div class="examplenum">

[Example 560](#example-560)

</div>

<div class="column">

    [
     ]
    
    [
     ]: /uri

</div>

<div class="column">

    <p>[
    ]</p>
    <p>[
    ]: /uri</p>

</div>

</div>

A [collapsed reference link](#collapsed-reference-link) consists of a
[link label](#link-label) that [matches](#matches) a [link reference
definition](#link-reference-definition) elsewhere in the document,
followed by the string `[]`. The contents of the first link label are
parsed as inlines, which are used as the link’s text. The link’s URI and
title are provided by the matching reference link definition. Thus,
`[foo][]` is equivalent to `[foo][foo]`.

<div id="example-561" class="example">

<div class="examplenum">

[Example 561](#example-561)

</div>

<div class="column">

    [foo][]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title">foo</a></p>

</div>

</div>

<div id="example-562" class="example">

<div class="examplenum">

[Example 562](#example-562)

</div>

<div class="column">

    [*foo* bar][]
    
    [*foo* bar]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title"><em>foo</em> bar</a></p>

</div>

</div>

The link labels are case-insensitive:

<div id="example-563" class="example">

<div class="examplenum">

[Example 563](#example-563)

</div>

<div class="column">

    [Foo][]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title">Foo</a></p>

</div>

</div>

As with full reference links, [whitespace](#whitespace) is not allowed
between the two sets of brackets:

<div id="example-564" class="example">

<div class="examplenum">

[Example 564](#example-564)

</div>

<div class="column">

    [foo] 
    []
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title">foo</a>
    []</p>

</div>

</div>

A [shortcut reference link](#shortcut-reference-link) consists of a
[link label](#link-label) that [matches](#matches) a [link reference
definition](#link-reference-definition) elsewhere in the document and is
not followed by `[]` or a link label. The contents of the first link
label are parsed as inlines, which are used as the link’s text. The
link’s URI and title are provided by the matching link reference
definition. Thus, `[foo]` is equivalent to `[foo][]`.

<div id="example-565" class="example">

<div class="examplenum">

[Example 565](#example-565)

</div>

<div class="column">

    [foo]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title">foo</a></p>

</div>

</div>

<div id="example-566" class="example">

<div class="examplenum">

[Example 566](#example-566)

</div>

<div class="column">

    [*foo* bar]
    
    [*foo* bar]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title"><em>foo</em> bar</a></p>

</div>

</div>

<div id="example-567" class="example">

<div class="examplenum">

[Example 567](#example-567)

</div>

<div class="column">

    [[*foo* bar]]
    
    [*foo* bar]: /url "title"

</div>

<div class="column">

    <p>[<a href="/url" title="title"><em>foo</em> bar</a>]</p>

</div>

</div>

<div id="example-568" class="example">

<div class="examplenum">

[Example 568](#example-568)

</div>

<div class="column">

    [[bar [foo]
    
    [foo]: /url

</div>

<div class="column">

    <p>[[bar <a href="/url">foo</a></p>

</div>

</div>

The link labels are case-insensitive:

<div id="example-569" class="example">

<div class="examplenum">

[Example 569](#example-569)

</div>

<div class="column">

    [Foo]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><a href="/url" title="title">Foo</a></p>

</div>

</div>

A space after the link text should be preserved:

<div id="example-570" class="example">

<div class="examplenum">

[Example 570](#example-570)

</div>

<div class="column">

    [foo] bar
    
    [foo]: /url

</div>

<div class="column">

    <p><a href="/url">foo</a> bar</p>

</div>

</div>

If you just want bracketed text, you can backslash-escape the opening
bracket to avoid links:

<div id="example-571" class="example">

<div class="examplenum">

[Example 571](#example-571)

</div>

<div class="column">

    \[foo]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p>[foo]</p>

</div>

</div>

Note that this is a link, because a link label ends with the first
following closing bracket:

<div id="example-572" class="example">

<div class="examplenum">

[Example 572](#example-572)

</div>

<div class="column">

    [foo*]: /url
    
    *[foo*]

</div>

<div class="column">

    <p>*<a href="/url">foo*</a></p>

</div>

</div>

Full and compact references take precedence over shortcut references:

<div id="example-573" class="example">

<div class="examplenum">

[Example 573](#example-573)

</div>

<div class="column">

    [foo][bar]
    
    [foo]: /url1
    [bar]: /url2

</div>

<div class="column">

    <p><a href="/url2">foo</a></p>

</div>

</div>

<div id="example-574" class="example">

<div class="examplenum">

[Example 574](#example-574)

</div>

<div class="column">

    [foo][]
    
    [foo]: /url1

</div>

<div class="column">

    <p><a href="/url1">foo</a></p>

</div>

</div>

Inline links also take precedence:

<div id="example-575" class="example">

<div class="examplenum">

[Example 575](#example-575)

</div>

<div class="column">

    [foo]()
    
    [foo]: /url1

</div>

<div class="column">

    <p><a href="">foo</a></p>

</div>

</div>

<div id="example-576" class="example">

<div class="examplenum">

[Example 576](#example-576)

</div>

<div class="column">

    [foo](not a link)
    
    [foo]: /url1

</div>

<div class="column">

    <p><a href="/url1">foo</a>(not a link)</p>

</div>

</div>

In the following case `[bar][baz]` is parsed as a reference, `[foo]` as
normal text:

<div id="example-577" class="example">

<div class="examplenum">

[Example 577](#example-577)

</div>

<div class="column">

    [foo][bar][baz]
    
    [baz]: /url

</div>

<div class="column">

    <p>[foo]<a href="/url">bar</a></p>

</div>

</div>

Here, though, `[foo][bar]` is parsed as a reference, since `[bar]` is
defined:

<div id="example-578" class="example">

<div class="examplenum">

[Example 578](#example-578)

</div>

<div class="column">

    [foo][bar][baz]
    
    [baz]: /url1
    [bar]: /url2

</div>

<div class="column">

    <p><a href="/url2">foo</a><a href="/url1">baz</a></p>

</div>

</div>

Here `[foo]` is not parsed as a shortcut reference, because it is
followed by a link label (even though `[bar]` is not defined):

<div id="example-579" class="example">

<div class="examplenum">

[Example 579](#example-579)

</div>

<div class="column">

    [foo][bar][baz]
    
    [baz]: /url1
    [foo]: /url2

</div>

<div class="column">

    <p>[foo]<a href="/url1">bar</a></p>

</div>

</div>

## [](#TOC)<span class="number">6.7</span>Images

Syntax for images is like the syntax for links, with one difference.
Instead of [link text](#link-text), we have an [image
description](#image-description). The rules for this are the same as for
[link text](#link-text), except that (a) an image description starts
with `![` rather than `[`, and (b) an image description may contain
links. An image description has inline elements as its contents. When an
image is rendered to HTML, this is standardly used as the image’s `alt`
attribute.

<div id="example-580" class="example">

<div class="examplenum">

[Example 580](#example-580)

</div>

<div class="column">

    ![foo](/url "title")

</div>

<div class="column">

    <p><img src="/url" alt="foo" title="title" /></p>

</div>

</div>

<div id="example-581" class="example">

<div class="examplenum">

[Example 581](#example-581)

</div>

<div class="column">

    ![foo *bar*]
    
    [foo *bar*]: train.jpg "train & tracks"

</div>

<div class="column">

    <p><img src="train.jpg" alt="foo bar" title="train &amp; tracks" /></p>

</div>

</div>

<div id="example-582" class="example">

<div class="examplenum">

[Example 582](#example-582)

</div>

<div class="column">

    ![foo ![bar](/url)](/url2)

</div>

<div class="column">

    <p><img src="/url2" alt="foo bar" /></p>

</div>

</div>

<div id="example-583" class="example">

<div class="examplenum">

[Example 583](#example-583)

</div>

<div class="column">

    ![foo [bar](/url)](/url2)

</div>

<div class="column">

    <p><img src="/url2" alt="foo bar" /></p>

</div>

</div>

Though this spec is concerned with parsing, not rendering, it is
recommended that in rendering to HTML, only the plain string content of
the [image description](#image-description) be used. Note that in the
above example, the alt attribute’s value is `foo bar`, not `foo
[bar](/url)` or `foo <a href="/url">bar</a>`. Only the plain string
content is rendered, without formatting.

<div id="example-584" class="example">

<div class="examplenum">

[Example 584](#example-584)

</div>

<div class="column">

    ![foo *bar*][]
    
    [foo *bar*]: train.jpg "train & tracks"

</div>

<div class="column">

    <p><img src="train.jpg" alt="foo bar" title="train &amp; tracks" /></p>

</div>

</div>

<div id="example-585" class="example">

<div class="examplenum">

[Example 585](#example-585)

</div>

<div class="column">

    ![foo *bar*][foobar]
    
    [FOOBAR]: train.jpg "train & tracks"

</div>

<div class="column">

    <p><img src="train.jpg" alt="foo bar" title="train &amp; tracks" /></p>

</div>

</div>

<div id="example-586" class="example">

<div class="examplenum">

[Example 586](#example-586)

</div>

<div class="column">

    ![foo](train.jpg)

</div>

<div class="column">

    <p><img src="train.jpg" alt="foo" /></p>

</div>

</div>

<div id="example-587" class="example">

<div class="examplenum">

[Example 587](#example-587)

</div>

<div class="column">

    My ![foo bar](/path/to/train.jpg  "title"   )

</div>

<div class="column">

    <p>My <img src="/path/to/train.jpg" alt="foo bar" title="title" /></p>

</div>

</div>

<div id="example-588" class="example">

<div class="examplenum">

[Example 588](#example-588)

</div>

<div class="column">

    ![foo](<url>)

</div>

<div class="column">

    <p><img src="url" alt="foo" /></p>

</div>

</div>

<div id="example-589" class="example">

<div class="examplenum">

[Example 589](#example-589)

</div>

<div class="column">

    ![](/url)

</div>

<div class="column">

    <p><img src="/url" alt="" /></p>

</div>

</div>

Reference-style:

<div id="example-590" class="example">

<div class="examplenum">

[Example 590](#example-590)

</div>

<div class="column">

    ![foo][bar]
    
    [bar]: /url

</div>

<div class="column">

    <p><img src="/url" alt="foo" /></p>

</div>

</div>

<div id="example-591" class="example">

<div class="examplenum">

[Example 591](#example-591)

</div>

<div class="column">

    ![foo][bar]
    
    [BAR]: /url

</div>

<div class="column">

    <p><img src="/url" alt="foo" /></p>

</div>

</div>

Collapsed:

<div id="example-592" class="example">

<div class="examplenum">

[Example 592](#example-592)

</div>

<div class="column">

    ![foo][]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><img src="/url" alt="foo" title="title" /></p>

</div>

</div>

<div id="example-593" class="example">

<div class="examplenum">

[Example 593](#example-593)

</div>

<div class="column">

    ![*foo* bar][]
    
    [*foo* bar]: /url "title"

</div>

<div class="column">

    <p><img src="/url" alt="foo bar" title="title" /></p>

</div>

</div>

The labels are case-insensitive:

<div id="example-594" class="example">

<div class="examplenum">

[Example 594](#example-594)

</div>

<div class="column">

    ![Foo][]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><img src="/url" alt="Foo" title="title" /></p>

</div>

</div>

As with reference links, [whitespace](#whitespace) is not allowed
between the two sets of brackets:

<div id="example-595" class="example">

<div class="examplenum">

[Example 595](#example-595)

</div>

<div class="column">

    ![foo] 
    []
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><img src="/url" alt="foo" title="title" />
    []</p>

</div>

</div>

Shortcut:

<div id="example-596" class="example">

<div class="examplenum">

[Example 596](#example-596)

</div>

<div class="column">

    ![foo]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><img src="/url" alt="foo" title="title" /></p>

</div>

</div>

<div id="example-597" class="example">

<div class="examplenum">

[Example 597](#example-597)

</div>

<div class="column">

    ![*foo* bar]
    
    [*foo* bar]: /url "title"

</div>

<div class="column">

    <p><img src="/url" alt="foo bar" title="title" /></p>

</div>

</div>

Note that link labels cannot contain unescaped brackets:

<div id="example-598" class="example">

<div class="examplenum">

[Example 598](#example-598)

</div>

<div class="column">

    ![[foo]]
    
    [[foo]]: /url "title"

</div>

<div class="column">

    <p>![[foo]]</p>
    <p>[[foo]]: /url &quot;title&quot;</p>

</div>

</div>

The link labels are case-insensitive:

<div id="example-599" class="example">

<div class="examplenum">

[Example 599](#example-599)

</div>

<div class="column">

    ![Foo]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p><img src="/url" alt="Foo" title="title" /></p>

</div>

</div>

If you just want a literal `!` followed by bracketed text, you can
backslash-escape the opening `[`:

<div id="example-600" class="example">

<div class="examplenum">

[Example 600](#example-600)

</div>

<div class="column">

    !\[foo]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p>![foo]</p>

</div>

</div>

If you want a link after a literal `!`, backslash-escape the `!`:

<div id="example-601" class="example">

<div class="examplenum">

[Example 601](#example-601)

</div>

<div class="column">

    \![foo]
    
    [foo]: /url "title"

</div>

<div class="column">

    <p>!<a href="/url" title="title">foo</a></p>

</div>

</div>

## [](#TOC)<span class="number">6.8</span>Autolinks

[Autolink](#autolink)s are absolute URIs and email addresses inside `<`
and `>`. They are parsed as links, with the URL or email address as the
link label.

A [URI autolink](#uri-autolink) consists of `<`, followed by an
[absolute URI](#absolute-uri) followed by `>`. It is parsed as a link to
the URI, with the URI as the link’s label.

An [absolute URI](#absolute-uri), for these purposes, consists of a
[scheme](#scheme) followed by a colon (`:`) followed by zero or more
characters other than ASCII [whitespace](#whitespace) and control
characters, `<`, and `>`. If the URI includes these characters, they
must be percent-encoded (e.g. `%20` for a space).

For purposes of this spec, a [scheme](#scheme) is any sequence of 2–32
characters beginning with an ASCII letter and followed by any
combination of ASCII letters, digits, or the symbols plus (”+”), period
(”.”), or hyphen (”-”).

Here are some valid autolinks:

<div id="example-602" class="example">

<div class="examplenum">

[Example 602](#example-602)

</div>

<div class="column">

    <http://foo.bar.baz>

</div>

<div class="column">

    <p><a href="http://foo.bar.baz">http://foo.bar.baz</a></p>

</div>

</div>

<div id="example-603" class="example">

<div class="examplenum">

[Example 603](#example-603)

</div>

<div class="column">

    <http://foo.bar.baz/test?q=hello&id=22&boolean>

</div>

<div class="column">

    <p><a href="http://foo.bar.baz/test?q=hello&amp;id=22&amp;boolean">http://foo.bar.baz/test?q=hello&amp;id=22&amp;boolean</a></p>

</div>

</div>

<div id="example-604" class="example">

<div class="examplenum">

[Example 604](#example-604)

</div>

<div class="column">

    <irc://foo.bar:2233/baz>

</div>

<div class="column">

    <p><a href="irc://foo.bar:2233/baz">irc://foo.bar:2233/baz</a></p>

</div>

</div>

Uppercase is also fine:

<div id="example-605" class="example">

<div class="examplenum">

[Example 605](#example-605)

</div>

<div class="column">

    <MAILTO:FOO@BAR.BAZ>

</div>

<div class="column">

    <p><a href="MAILTO:FOO@BAR.BAZ">MAILTO:FOO@BAR.BAZ</a></p>

</div>

</div>

Note that many strings that count as [absolute URIs](#absolute-uri) for
purposes of this spec are not valid URIs, because their schemes are not
registered or because of other problems with their syntax:

<div id="example-606" class="example">

<div class="examplenum">

[Example 606](#example-606)

</div>

<div class="column">

    <a+b+c:d>

</div>

<div class="column">

    <p><a href="a+b+c:d">a+b+c:d</a></p>

</div>

</div>

<div id="example-607" class="example">

<div class="examplenum">

[Example 607](#example-607)

</div>

<div class="column">

    <made-up-scheme://foo,bar>

</div>

<div class="column">

    <p><a href="made-up-scheme://foo,bar">made-up-scheme://foo,bar</a></p>

</div>

</div>

<div id="example-608" class="example">

<div class="examplenum">

[Example 608](#example-608)

</div>

<div class="column">

    <http://../>

</div>

<div class="column">

    <p><a href="http://../">http://../</a></p>

</div>

</div>

<div id="example-609" class="example">

<div class="examplenum">

[Example 609](#example-609)

</div>

<div class="column">

    <localhost:5001/foo>

</div>

<div class="column">

    <p><a href="localhost:5001/foo">localhost:5001/foo</a></p>

</div>

</div>

Spaces are not allowed in autolinks:

<div id="example-610" class="example">

<div class="examplenum">

[Example 610](#example-610)

</div>

<div class="column">

    <http://foo.bar/baz bim>

</div>

<div class="column">

    <p>&lt;http://foo.bar/baz bim&gt;</p>

</div>

</div>

Backslash-escapes do not work inside autolinks:

<div id="example-611" class="example">

<div class="examplenum">

[Example 611](#example-611)

</div>

<div class="column">

    <http://example.com/\[\>

</div>

<div class="column">

    <p><a href="http://example.com/%5C%5B%5C">http://example.com/\[\</a></p>

</div>

</div>

An [email autolink](#email-autolink) consists of `<`, followed by an
[email address](#email-address), followed by `>`. The link’s label is
the email address, and the URL is `mailto:` followed by the email
address.

An [email address](#email-address), for these purposes, is anything that
matches the [non-normative regex from the HTML5
spec](https://html.spec.whatwg.org/multipage/forms.html#e-mail-state-\(type=email\)):

    /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?
    (?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/

Examples of email autolinks:

<div id="example-612" class="example">

<div class="examplenum">

[Example 612](#example-612)

</div>

<div class="column">

    <foo@bar.example.com>

</div>

<div class="column">

    <p><a href="mailto:foo@bar.example.com">foo@bar.example.com</a></p>

</div>

</div>

<div id="example-613" class="example">

<div class="examplenum">

[Example 613](#example-613)

</div>

<div class="column">

    <foo+special@Bar.baz-bar0.com>

</div>

<div class="column">

    <p><a href="mailto:foo+special@Bar.baz-bar0.com">foo+special@Bar.baz-bar0.com</a></p>

</div>

</div>

Backslash-escapes do not work inside email autolinks:

<div id="example-614" class="example">

<div class="examplenum">

[Example 614](#example-614)

</div>

<div class="column">

    <foo\+@bar.example.com>

</div>

<div class="column">

    <p>&lt;foo+@bar.example.com&gt;</p>

</div>

</div>

These are not autolinks:

<div id="example-615" class="example">

<div class="examplenum">

[Example 615](#example-615)

</div>

<div class="column">

``` 
<>
```

</div>

<div class="column">

    <p>&lt;&gt;</p>

</div>

</div>

<div id="example-616" class="example">

<div class="examplenum">

[Example 616](#example-616)

</div>

<div class="column">

    < http://foo.bar >

</div>

<div class="column">

    <p>&lt; http://foo.bar &gt;</p>

</div>

</div>

<div id="example-617" class="example">

<div class="examplenum">

[Example 617](#example-617)

</div>

<div class="column">

    <m:abc>

</div>

<div class="column">

    <p>&lt;m:abc&gt;</p>

</div>

</div>

<div id="example-618" class="example">

<div class="examplenum">

[Example 618](#example-618)

</div>

<div class="column">

    <foo.bar.baz>

</div>

<div class="column">

    <p>&lt;foo.bar.baz&gt;</p>

</div>

</div>

<div id="example-619" class="example">

<div class="examplenum">

[Example 619](#example-619)

</div>

<div class="column">

    http://example.com

</div>

<div class="column">

    <p>http://example.com</p>

</div>

</div>

<div id="example-620" class="example">

<div class="examplenum">

[Example 620](#example-620)

</div>

<div class="column">

    foo@bar.example.com

</div>

<div class="column">

    <p>foo@bar.example.com</p>

</div>

</div>

<div class="extension">

## [](#TOC)<span class="number">6.9</span>Autolinks (extension)

GFM enables the `autolink` extension, where autolinks will be recognised
in a greater number of conditions.

[Autolink](#autolink)s can also be constructed without requiring the use
of `<` and to `>` to delimit them, although they will be recognized
under a smaller set of circumstances. All such recognized autolinks can
only come at the beginning of a line, after whitespace, or any of the
delimiting characters `*`, `_`, `~`, and `(`.

An [extended www autolink](#extended-www-autolink) will be recognized
when the text `www.` is found followed by a [valid
domain](#valid-domain). A [valid domain](#valid-domain) consists of
segments of alphanumeric characters, underscores (`_`) and hyphens (`-`)
separated by periods (`.`). There must be at least one period, and no
underscores may be present in the last two segments of the domain.

The scheme `http` will be inserted automatically:

<div id="example-621" class="example">

<div class="examplenum">

[Example 621](#example-621)

</div>

<div class="column">

    www.commonmark.org

</div>

<div class="column">

    <p><a href="http://www.commonmark.org">www.commonmark.org</a></p>

</div>

</div>

After a [valid domain](#valid-domain), zero or more non-space non-`<`
characters may follow:

<div id="example-622" class="example">

<div class="examplenum">

[Example 622](#example-622)

</div>

<div class="column">

    Visit www.commonmark.org/help for more information.

</div>

<div class="column">

    <p>Visit <a href="http://www.commonmark.org/help">www.commonmark.org/help</a> for more information.</p>

</div>

</div>

We then apply [extended autolink path
validation](#extended-autolink-path-validation) as follows:

Trailing punctuation (specifically, `?`, `!`, `.`, `,`, `:`, `*`, `_`,
and `~`) will not be considered part of the autolink, though they may be
included in the interior of the link:

<div id="example-623" class="example">

<div class="examplenum">

[Example 623](#example-623)

</div>

<div class="column">

    Visit www.commonmark.org.
    
    Visit www.commonmark.org/a.b.

</div>

<div class="column">

    <p>Visit <a href="http://www.commonmark.org">www.commonmark.org</a>.</p>
    <p>Visit <a href="http://www.commonmark.org/a.b">www.commonmark.org/a.b</a>.</p>

</div>

</div>

When an autolink ends in `)`, we scan the entire autolink for the total
number of parentheses. If there is a greater number of closing
parentheses than opening ones, we don’t consider the unmatched trailing
parentheses part of the autolink, in order to facilitate including an
autolink inside a parenthesis:

<div id="example-624" class="example">

<div class="examplenum">

[Example 624](#example-624)

</div>

<div class="column">

    www.google.com/search?q=Markup+(business)
    
    www.google.com/search?q=Markup+(business)))
    
    (www.google.com/search?q=Markup+(business))
    
    (www.google.com/search?q=Markup+(business)

</div>

<div class="column">

    <p><a href="http://www.google.com/search?q=Markup+(business)">www.google.com/search?q=Markup+(business)</a></p>
    <p><a href="http://www.google.com/search?q=Markup+(business)">www.google.com/search?q=Markup+(business)</a>))</p>
    <p>(<a href="http://www.google.com/search?q=Markup+(business)">www.google.com/search?q=Markup+(business)</a>)</p>
    <p>(<a href="http://www.google.com/search?q=Markup+(business)">www.google.com/search?q=Markup+(business)</a></p>

</div>

</div>

This check is only done when the link ends in a closing parentheses `)`,
so if the only parentheses are in the interior of the autolink, no
special rules are applied:

<div id="example-625" class="example">

<div class="examplenum">

[Example 625](#example-625)

</div>

<div class="column">

    www.google.com/search?q=(business))+ok

</div>

<div class="column">

    <p><a href="http://www.google.com/search?q=(business))+ok">www.google.com/search?q=(business))+ok</a></p>

</div>

</div>

If an autolink ends in a semicolon (`;`), we check to see if it appears
to resemble an [entity reference](#entity-references); if the preceding
text is `&` followed by one or more alphanumeric characters. If so, it
is excluded from the autolink:

<div id="example-626" class="example">

<div class="examplenum">

[Example 626](#example-626)

</div>

<div class="column">

    www.google.com/search?q=commonmark&hl=en
    
    www.google.com/search?q=commonmark&hl;

</div>

<div class="column">

    <p><a href="http://www.google.com/search?q=commonmark&amp;hl=en">www.google.com/search?q=commonmark&amp;hl=en</a></p>
    <p><a href="http://www.google.com/search?q=commonmark">www.google.com/search?q=commonmark</a>&amp;hl;</p>

</div>

</div>

`<` immediately ends an autolink.

<div id="example-627" class="example">

<div class="examplenum">

[Example 627](#example-627)

</div>

<div class="column">

    www.commonmark.org/he<lp

</div>

<div class="column">

    <p><a href="http://www.commonmark.org/he">www.commonmark.org/he</a>&lt;lp</p>

</div>

</div>

An [extended url autolink](#extended-url-autolink) will be recognised
when one of the schemes `http://`, or `https://`, followed by a [valid
domain](#valid-domain), then zero or more non-space non-`<` characters
according to [extended autolink path
validation](#extended-autolink-path-validation):

<div id="example-628" class="example">

<div class="examplenum">

[Example 628](#example-628)

</div>

<div class="column">

    http://commonmark.org
    
    (Visit https://encrypted.google.com/search?q=Markup+(business))

</div>

<div class="column">

    <p><a href="http://commonmark.org">http://commonmark.org</a></p>
    <p>(Visit <a href="https://encrypted.google.com/search?q=Markup+(business)">https://encrypted.google.com/search?q=Markup+(business)</a>)</p>

</div>

</div>

An [extended email autolink](#extended-email-autolink) will be
recognised when an email address is recognised within any text node.
Email addresses are recognised according to the following rules:

  - One ore more characters which are alphanumeric, or `.`, `-`, `_`, or
    `+`.
  - An `@` symbol.
  - One or more characters which are alphanumeric, or `-` or `_`,
    separated by periods (`.`). There must be at least one period. The
    last character must not be one of `-` or `_`.

The scheme `mailto:` will automatically be added to the generated link:

<div id="example-629" class="example">

<div class="examplenum">

[Example 629](#example-629)

</div>

<div class="column">

    foo@bar.baz

</div>

<div class="column">

    <p><a href="mailto:foo@bar.baz">foo@bar.baz</a></p>

</div>

</div>

`+` can occur before the `@`, but not after.

<div id="example-630" class="example">

<div class="examplenum">

[Example 630](#example-630)

</div>

<div class="column">

    hello@mail+xyz.example isn't valid, but hello+xyz@mail.example is.

</div>

<div class="column">

    <p>hello@mail+xyz.example isn't valid, but <a href="mailto:hello+xyz@mail.example">hello+xyz@mail.example</a> is.</p>

</div>

</div>

`.`, `-`, and `_` can occur on both sides of the `@`, but only `.` may
occur at the end of the email address, in which case it will not be
considered part of the address:

<div id="example-631" class="example">

<div class="examplenum">

[Example 631](#example-631)

</div>

<div class="column">

    a.b-c_d@a.b
    
    a.b-c_d@a.b.
    
    a.b-c_d@a.b-
    
    a.b-c_d@a.b_

</div>

<div class="column">

    <p><a href="mailto:a.b-c_d@a.b">a.b-c_d@a.b</a></p>
    <p><a href="mailto:a.b-c_d@a.b">a.b-c_d@a.b</a>.</p>
    <p>a.b-c_d@a.b-</p>
    <p>a.b-c_d@a.b_</p>

</div>

</div>

</div>

## [](#TOC)<span class="number">6.10</span>Raw HTML

Text between `<` and `>` that looks like an HTML tag is parsed as a raw
HTML tag and will be rendered in HTML without escaping. Tag and
attribute names are not limited to current HTML tags, so custom tags
(and even, say, DocBook tags) may be used.

Here is the grammar for tags:

A [tag name](#tag-name) consists of an ASCII letter followed by zero or
more ASCII letters, digits, or hyphens (`-`).

An [attribute](#attribute) consists of [whitespace](#whitespace), an
[attribute name](#attribute-name), and an optional [attribute value
specification](#attribute-value-specification).

An [attribute name](#attribute-name) consists of an ASCII letter, `_`,
or `:`, followed by zero or more ASCII letters, digits, `_`, `.`, `:`,
or `-`. (Note: This is the XML specification restricted to ASCII. HTML5
is laxer.)

An [attribute value specification](#attribute-value-specification)
consists of optional [whitespace](#whitespace), a `=` character,
optional [whitespace](#whitespace), and an [attribute
value](#attribute-value).

An [attribute value](#attribute-value) consists of an [unquoted
attribute value](#unquoted-attribute-value), a [single-quoted attribute
value](#single-quoted-attribute-value), or a [double-quoted attribute
value](#double-quoted-attribute-value).

An [unquoted attribute value](#unquoted-attribute-value) is a nonempty
string of characters not including [whitespace](#whitespace), `"`, `'`,
`=`, `<`, `>`, or `` ` ``.

A [single-quoted attribute value](#single-quoted-attribute-value)
consists of `'`, zero or more characters not including `'`, and a final
`'`.

A [double-quoted attribute value](#double-quoted-attribute-value)
consists of `"`, zero or more characters not including `"`, and a final
`"`.

An [open tag](#open-tag) consists of a `<` character, a [tag
name](#tag-name), zero or more [attributes](#attribute), optional
[whitespace](#whitespace), an optional `/` character, and a `>`
character.

A [closing tag](#closing-tag) consists of the string `</`, a [tag
name](#tag-name), optional [whitespace](#whitespace), and the character
`>`.

An [HTML comment](#html-comment) consists of `<!--` + *text* + `-->`,
where *text* does not start with `>` or `->`, does not end with `-`, and
does not contain `--`. (See the [HTML5
spec](http://www.w3.org/TR/html5/syntax.html#comments).)

A [processing instruction](#processing-instruction) consists of the
string `<?`, a string of characters not including the string `?>`, and
the string `?>`.

A [declaration](#declaration) consists of the string `<!`, a name
consisting of one or more uppercase ASCII letters,
[whitespace](#whitespace), a string of characters not including the
character `>`, and the character `>`.

A [CDATA section](#cdata-section) consists of the string `<![CDATA[`, a
string of characters not including the string `]]>`, and the string
`]]>`.

An [HTML tag](#html-tag) consists of an [open tag](#open-tag), a
[closing tag](#closing-tag), an [HTML comment](#html-comment), a
[processing instruction](#processing-instruction), a
[declaration](#declaration), or a [CDATA section](#cdata-section).

Here are some simple open tags:

<div id="example-632" class="example">

<div class="examplenum">

[Example 632](#example-632)

</div>

<div class="column">

    <a><bab><c2c>

</div>

<div class="column">

    <p><a><bab><c2c></p>

</div>

</div>

Empty elements:

<div id="example-633" class="example">

<div class="examplenum">

[Example 633](#example-633)

</div>

<div class="column">

    <a/><b2/>

</div>

<div class="column">

    <p><a/><b2/></p>

</div>

</div>

[Whitespace](#whitespace) is allowed:

<div id="example-634" class="example">

<div class="examplenum">

[Example 634](#example-634)

</div>

<div class="column">

    <a  /><b2
    data="foo" >

</div>

<div class="column">

    <p><a  /><b2
    data="foo" ></p>

</div>

</div>

With attributes:

<div id="example-635" class="example">

<div class="examplenum">

[Example 635](#example-635)

</div>

<div class="column">

    <a foo="bar" bam = 'baz <em>"</em>'
    _boolean zoop:33=zoop:33 />

</div>

<div class="column">

    <p><a foo="bar" bam = 'baz <em>"</em>'
    _boolean zoop:33=zoop:33 /></p>

</div>

</div>

Custom tag names can be used:

<div id="example-636" class="example">

<div class="examplenum">

[Example 636](#example-636)

</div>

<div class="column">

    Foo <responsive-image src="foo.jpg" />

</div>

<div class="column">

    <p>Foo <responsive-image src="foo.jpg" /></p>

</div>

</div>

Illegal tag names, not parsed as HTML:

<div id="example-637" class="example">

<div class="examplenum">

[Example 637](#example-637)

</div>

<div class="column">

    <33> <__>

</div>

<div class="column">

    <p>&lt;33&gt; &lt;__&gt;</p>

</div>

</div>

Illegal attribute names:

<div id="example-638" class="example">

<div class="examplenum">

[Example 638](#example-638)

</div>

<div class="column">

    <a h*#ref="hi">

</div>

<div class="column">

    <p>&lt;a h*#ref=&quot;hi&quot;&gt;</p>

</div>

</div>

Illegal attribute values:

<div id="example-639" class="example">

<div class="examplenum">

[Example 639](#example-639)

</div>

<div class="column">

    <a href="hi'> <a href=hi'>

</div>

<div class="column">

    <p>&lt;a href=&quot;hi'&gt; &lt;a href=hi'&gt;</p>

</div>

</div>

Illegal [whitespace](#whitespace):

<div id="example-640" class="example">

<div class="examplenum">

[Example 640](#example-640)

</div>

<div class="column">

    < a><
    foo><bar/ >
    <foo bar=baz
    bim!bop />

</div>

<div class="column">

    <p>&lt; a&gt;&lt;
    foo&gt;&lt;bar/ &gt;
    &lt;foo bar=baz
    bim!bop /&gt;</p>

</div>

</div>

Missing [whitespace](#whitespace):

<div id="example-641" class="example">

<div class="examplenum">

[Example 641](#example-641)

</div>

<div class="column">

    <a href='bar'title=title>

</div>

<div class="column">

    <p>&lt;a href='bar'title=title&gt;</p>

</div>

</div>

Closing tags:

<div id="example-642" class="example">

<div class="examplenum">

[Example 642](#example-642)

</div>

<div class="column">

    </a></foo >

</div>

<div class="column">

    <p></a></foo ></p>

</div>

</div>

Illegal attributes in closing tag:

<div id="example-643" class="example">

<div class="examplenum">

[Example 643](#example-643)

</div>

<div class="column">

    </a href="foo">

</div>

<div class="column">

    <p>&lt;/a href=&quot;foo&quot;&gt;</p>

</div>

</div>

Comments:

<div id="example-644" class="example">

<div class="examplenum">

[Example 644](#example-644)

</div>

<div class="column">

    foo <!-- this is a
    comment - with hyphen -->

</div>

<div class="column">

    <p>foo <!-- this is a
    comment - with hyphen --></p>

</div>

</div>

<div id="example-645" class="example">

<div class="examplenum">

[Example 645](#example-645)

</div>

<div class="column">

    foo <!-- not a comment -- two hyphens -->

</div>

<div class="column">

    <p>foo &lt;!-- not a comment -- two hyphens --&gt;</p>

</div>

</div>

Not comments:

<div id="example-646" class="example">

<div class="examplenum">

[Example 646](#example-646)

</div>

<div class="column">

    foo <!--> foo -->
    
    foo <!-- foo--->

</div>

<div class="column">

    <p>foo &lt;!--&gt; foo --&gt;</p>
    <p>foo &lt;!-- foo---&gt;</p>

</div>

</div>

Processing instructions:

<div id="example-647" class="example">

<div class="examplenum">

[Example 647](#example-647)

</div>

<div class="column">

    foo <?php echo $a; ?>

</div>

<div class="column">

    <p>foo <?php echo $a; ?></p>

</div>

</div>

Declarations:

<div id="example-648" class="example">

<div class="examplenum">

[Example 648](#example-648)

</div>

<div class="column">

    foo <!ELEMENT br EMPTY>

</div>

<div class="column">

    <p>foo <!ELEMENT br EMPTY></p>

</div>

</div>

CDATA sections:

<div id="example-649" class="example">

<div class="examplenum">

[Example 649](#example-649)

</div>

<div class="column">

    foo <![CDATA[>&<]]>

</div>

<div class="column">

    <p>foo <![CDATA[>&<]]></p>

</div>

</div>

Entity and numeric character references are preserved in HTML
attributes:

<div id="example-650" class="example">

<div class="examplenum">

[Example 650](#example-650)

</div>

<div class="column">

    foo <a href="&ouml;">

</div>

<div class="column">

    <p>foo <a href="&ouml;"></p>

</div>

</div>

Backslash escapes do not work in HTML attributes:

<div id="example-651" class="example">

<div class="examplenum">

[Example 651](#example-651)

</div>

<div class="column">

    foo <a href="\*">

</div>

<div class="column">

    <p>foo <a href="\*"></p>

</div>

</div>

<div id="example-652" class="example">

<div class="examplenum">

[Example 652](#example-652)

</div>

<div class="column">

    <a href="\"">

</div>

<div class="column">

    <p>&lt;a href=&quot;&quot;&quot;&gt;</p>

</div>

</div>

<div class="extension">

## [](#TOC)<span class="number">6.11</span>Disallowed Raw HTML (extension)

GFM enables the `tagfilter` extension, where the following HTML tags
will be filtered when rendering HTML output:

  - `<title>`
  - `<textarea>`
  - `<style>`
  - `<xmp>`
  - `<iframe>`
  - `<noembed>`
  - `<noframes>`
  - `<script>`
  - `<plaintext>`

Filtering is done by replacing the leading `<` with the entity `&lt;`.
These tags are chosen in particular as they change how HTML is
interpreted in a way unique to them (i.e. nested HTML is interpreted
differently), and this is usually undesireable in the context of other
rendered Markdown content.

All other HTML tags are left untouched.

<div id="example-653" class="example">

<div class="examplenum">

[Example 653](#example-653)

</div>

<div class="column">

    <strong> <title> <style> <em>
    
    <blockquote>
      <xmp> is disallowed.  <XMP> is also disallowed.
    </blockquote>

</div>

<div class="column">

    <p><strong> &lt;title> &lt;style> <em></p>
    <blockquote>
      &lt;xmp> is disallowed.  &lt;XMP> is also disallowed.
    </blockquote>

</div>

</div>

</div>

## [](#TOC)<span class="number">6.12</span>Hard line breaks

A line break (not in a code span or HTML tag) that is preceded by two or
more spaces and does not occur at the end of a block is parsed as a
[hard line break](#hard-line-break) (rendered in HTML as a `<br />`
tag):

<div id="example-654" class="example">

<div class="examplenum">

[Example 654](#example-654)

</div>

<div class="column">

    foo  
    baz

</div>

<div class="column">

    <p>foo<br />
    baz</p>

</div>

</div>

For a more visible alternative, a backslash before the [line
ending](#line-ending) may be used instead of two spaces:

<div id="example-655" class="example">

<div class="examplenum">

[Example 655](#example-655)

</div>

<div class="column">

    foo\
    baz

</div>

<div class="column">

    <p>foo<br />
    baz</p>

</div>

</div>

More than two spaces can be used:

<div id="example-656" class="example">

<div class="examplenum">

[Example 656](#example-656)

</div>

<div class="column">

    foo       
    baz

</div>

<div class="column">

    <p>foo<br />
    baz</p>

</div>

</div>

Leading spaces at the beginning of the next line are ignored:

<div id="example-657" class="example">

<div class="examplenum">

[Example 657](#example-657)

</div>

<div class="column">

    foo  
         bar

</div>

<div class="column">

    <p>foo<br />
    bar</p>

</div>

</div>

<div id="example-658" class="example">

<div class="examplenum">

[Example 658](#example-658)

</div>

<div class="column">

    foo\
         bar

</div>

<div class="column">

    <p>foo<br />
    bar</p>

</div>

</div>

Line breaks can occur inside emphasis, links, and other constructs that
allow inline content:

<div id="example-659" class="example">

<div class="examplenum">

[Example 659](#example-659)

</div>

<div class="column">

    *foo  
    bar*

</div>

<div class="column">

    <p><em>foo<br />
    bar</em></p>

</div>

</div>

<div id="example-660" class="example">

<div class="examplenum">

[Example 660](#example-660)

</div>

<div class="column">

    *foo\
    bar*

</div>

<div class="column">

    <p><em>foo<br />
    bar</em></p>

</div>

</div>

Line breaks do not occur inside code spans

<div id="example-661" class="example">

<div class="examplenum">

[Example 661](#example-661)

</div>

<div class="column">

    `code  
    span`

</div>

<div class="column">

    <p><code>code   span</code></p>

</div>

</div>

<div id="example-662" class="example">

<div class="examplenum">

[Example 662](#example-662)

</div>

<div class="column">

    `code\
    span`

</div>

<div class="column">

    <p><code>code\ span</code></p>

</div>

</div>

or HTML tags:

<div id="example-663" class="example">

<div class="examplenum">

[Example 663](#example-663)

</div>

<div class="column">

    <a href="foo  
    bar">

</div>

<div class="column">

    <p><a href="foo  
    bar"></p>

</div>

</div>

<div id="example-664" class="example">

<div class="examplenum">

[Example 664](#example-664)

</div>

<div class="column">

    <a href="foo\
    bar">

</div>

<div class="column">

    <p><a href="foo\
    bar"></p>

</div>

</div>

Hard line breaks are for separating inline content within a block.
Neither syntax for hard line breaks works at the end of a paragraph or
other block element:

<div id="example-665" class="example">

<div class="examplenum">

[Example 665](#example-665)

</div>

<div class="column">

    foo\

</div>

<div class="column">

    <p>foo\</p>

</div>

</div>

<div id="example-666" class="example">

<div class="examplenum">

[Example 666](#example-666)

</div>

<div class="column">

``` 
foo  
```

</div>

<div class="column">

    <p>foo</p>

</div>

</div>

<div id="example-667" class="example">

<div class="examplenum">

[Example 667](#example-667)

</div>

<div class="column">

    ### foo\

</div>

<div class="column">

    <h3>foo\</h3>

</div>

</div>

<div id="example-668" class="example">

<div class="examplenum">

[Example 668](#example-668)

</div>

<div class="column">

``` 
### foo  
```

</div>

<div class="column">

    <h3>foo</h3>

</div>

</div>

## [](#TOC)<span class="number">6.13</span>Soft line breaks

A regular line break (not in a code span or HTML tag) that is not
preceded by two or more spaces or a backslash is parsed as a
[softbreak](#softbreak). (A softbreak may be rendered in HTML either as
a [line ending](#line-ending) or as a space. The result will be the same
in browsers. In the examples here, a [line ending](#line-ending) will be
used.)

<div id="example-669" class="example">

<div class="examplenum">

[Example 669](#example-669)

</div>

<div class="column">

    foo
    baz

</div>

<div class="column">

    <p>foo
    baz</p>

</div>

</div>

Spaces at the end of the line and beginning of the next line are
removed:

<div id="example-670" class="example">

<div class="examplenum">

[Example 670](#example-670)

</div>

<div class="column">

    foo 
     baz

</div>

<div class="column">

    <p>foo
    baz</p>

</div>

</div>

A conforming parser may render a soft line break in HTML either as a
line break or as a space.

A renderer may also provide an option to render soft line breaks as hard
line breaks.

## [](#TOC)<span class="number">6.14</span>Textual content

Any characters not given an interpretation by the above rules will be
parsed as plain textual content.

<div id="example-671" class="example">

<div class="examplenum">

[Example 671](#example-671)

</div>

<div class="column">

    hello $.;'there

</div>

<div class="column">

    <p>hello $.;'there</p>

</div>

</div>

<div id="example-672" class="example">

<div class="examplenum">

[Example 672](#example-672)

</div>

<div class="column">

    Foo χρῆν

</div>

<div class="column">

    <p>Foo χρῆν</p>

</div>

</div>

Internal spaces are preserved verbatim:

<div id="example-673" class="example">

<div class="examplenum">

[Example 673](#example-673)

</div>

<div class="column">

    Multiple     spaces

</div>

<div class="column">

    <p>Multiple     spaces</p>

</div>

</div>

<div class="appendices">

# Appendix: A parsing strategy

</div>

In this appendix we describe some features of the parsing strategy used
in the CommonMark reference implementations.

## Overview

Parsing has two phases:

1.  In the first phase, lines of input are consumed and the block
    structure of the document—its division into paragraphs, block
    quotes, list items, and so on—is constructed. Text is assigned to
    these blocks but not parsed. Link reference definitions are parsed
    and a map of links is constructed.

2.  In the second phase, the raw text contents of paragraphs and
    headings are parsed into sequences of Markdown inline elements
    (strings, code spans, links, emphasis, and so on), using the map of
    link references constructed in phase 1.

At each point in processing, the document is represented as a tree of
**blocks**. The root of the tree is a `document` block. The `document`
may have any number of other blocks as **children**. These children may,
in turn, have other blocks as children. The last child of a block is
normally considered **open**, meaning that subsequent lines of input can
alter its contents. (Blocks that are not open are **closed**.) Here, for
example, is a possible document tree, with the open blocks marked by
arrows:

    -> document
      -> block_quote
           paragraph
             "Lorem ipsum dolor\nsit amet."
        -> list (type=bullet tight=true bullet_char=-)
             list_item
               paragraph
                 "Qui *quodsi iracundia*"
          -> list_item
            -> paragraph
                 "aliquando id"

## Phase 1: block structure

Each line that is processed has an effect on this tree. The line is
analyzed and, depending on its contents, the document may be altered in
one or more of the following ways:

1.  One or more open blocks may be closed.
2.  One or more new blocks may be created as children of the last open
    block.
3.  Text may be added to the last (deepest) open block remaining on the
    tree.

Once a line has been incorporated into the tree in this way, it can be
discarded, so input can be read in a stream.

For each line, we follow this procedure:

1.  First we iterate through the open blocks, starting with the root
    document, and descending through last children down to the last open
    block. Each block imposes a condition that the line must satisfy if
    the block is to remain open. For example, a block quote requires a
    `>` character. A paragraph requires a non-blank line. In this phase
    we may match all or just some of the open blocks. But we cannot
    close unmatched blocks yet, because we may have a [lazy continuation
    line](#lazy-continuation-line).

2.  Next, after consuming the continuation markers for existing blocks,
    we look for new block starts (e.g. `>` for a block quote). If we
    encounter a new block start, we close any blocks unmatched in step 1
    before creating the new block as a child of the last matched block.

3.  Finally, we look at the remainder of the line (after block markers
    like `>`, list markers, and indentation have been consumed). This is
    text that can be incorporated into the last open block (a paragraph,
    code block, heading, or raw HTML).

Setext headings are formed when we see a line of a paragraph that is a
[setext heading underline](#setext-heading-underline).

Reference link definitions are detected when a paragraph is closed; the
accumulated text lines are parsed to see if they begin with one or more
reference link definitions. Any remainder becomes a normal paragraph.

We can see how this works by considering how the tree above is generated
by four lines of Markdown:

    > Lorem ipsum dolor
    sit amet.
    > - Qui *quodsi iracundia*
    > - aliquando id

At the outset, our document model is just

    -> document

The first line of our text,

    > Lorem ipsum dolor

causes a `block_quote` block to be created as a child of our open
`document` block, and a `paragraph` block as a child of the
`block_quote`. Then the text is added to the last open block, the
`paragraph`:

    -> document
      -> block_quote
        -> paragraph
             "Lorem ipsum dolor"

The next line,

    sit amet.

is a “lazy continuation” of the open `paragraph`, so it gets added to
the paragraph’s text:

    -> document
      -> block_quote
        -> paragraph
             "Lorem ipsum dolor\nsit amet."

The third line,

    > - Qui *quodsi iracundia*

causes the `paragraph` block to be closed, and a new `list` block opened
as a child of the `block_quote`. A `list_item` is also added as a child
of the `list`, and a `paragraph` as a child of the `list_item`. The text
is then added to the new `paragraph`:

    -> document
      -> block_quote
           paragraph
             "Lorem ipsum dolor\nsit amet."
        -> list (type=bullet tight=true bullet_char=-)
          -> list_item
            -> paragraph
                 "Qui *quodsi iracundia*"

The fourth line,

    > - aliquando id

causes the `list_item` (and its child the `paragraph`) to be closed, and
a new `list_item` opened up as child of the `list`. A `paragraph` is
added as a child of the new `list_item`, to contain the text. We thus
obtain the final tree:

    -> document
      -> block_quote
           paragraph
             "Lorem ipsum dolor\nsit amet."
        -> list (type=bullet tight=true bullet_char=-)
             list_item
               paragraph
                 "Qui *quodsi iracundia*"
          -> list_item
            -> paragraph
                 "aliquando id"

## Phase 2: inline structure

Once all of the input has been parsed, all open blocks are closed.

We then “walk the tree,” visiting every node, and parse raw string
contents of paragraphs and headings as inlines. At this point we have
seen all the link reference definitions, so we can resolve reference
links as we go.

    document
      block_quote
        paragraph
          str "Lorem ipsum dolor"
          softbreak
          str "sit amet."
        list (type=bullet tight=true bullet_char=-)
          list_item
            paragraph
              str "Qui "
              emph
                str "quodsi iracundia"
          list_item
            paragraph
              str "aliquando id"

Notice how the [line ending](#line-ending) in the first paragraph has
been parsed as a `softbreak`, and the asterisks in the first list item
have become an `emph`.

### An algorithm for parsing nested emphasis and links

By far the trickiest part of inline parsing is handling emphasis, strong
emphasis, links, and images. This is done using the following algorithm.

When we’re parsing inlines and we hit either

  - a run of `*` or `_` characters, or
  - a `[` or `![`

we insert a text node with these symbols as its literal content, and we
add a pointer to this text node to the [delimiter
stack](#delimiter-stack).

The [delimiter stack](#delimiter-stack) is a doubly linked list. Each
element contains a pointer to a text node, plus information about

  - the type of delimiter (`[`, `![`, `*`, `_`)
  - the number of delimiters,
  - whether the delimiter is “active” (all are active to start), and
  - whether the delimiter is a potential opener, a potential closer, or
    both (which depends on what sort of characters precede and follow
    the delimiters).

When we hit a `]` character, we call the *look for link or image*
procedure (see below).

When we hit the end of the input, we call the *process emphasis*
procedure (see below), with `stack_bottom` = NULL.

#### *look for link or image*

Starting at the top of the delimiter stack, we look backwards through
the stack for an opening `[` or `![` delimiter.

  - If we don’t find one, we return a literal text node `]`.

  - If we do find one, but it’s not *active*, we remove the inactive
    delimiter from the stack, and return a literal text node `]`.

  - If we find one and it’s active, then we parse ahead to see if we
    have an inline link/image, reference link/image, compact reference
    link/image, or shortcut reference link/image.
    
      - If we don’t, then we remove the opening delimiter from the
        delimiter stack and return a literal text node `]`.
    
      - If we do, then
        
          - We return a link or image node whose children are the
            inlines after the text node pointed to by the opening
            delimiter.
        
          - We run *process emphasis* on these inlines, with the `[`
            opener as `stack_bottom`.
        
          - We remove the opening delimiter.
        
          - If we have a link (and not an image), we also set all `[`
            delimiters before the opening delimiter to *inactive*. (This
            will prevent us from getting links within links.)

#### *process emphasis*

Parameter `stack_bottom` sets a lower bound to how far we descend in the
[delimiter stack](#delimiter-stack). If it is NULL, we can go all the
way to the bottom. Otherwise, we stop before visiting `stack_bottom`.

Let `current_position` point to the element on the [delimiter
stack](#delimiter-stack) just above `stack_bottom` (or the first element
if `stack_bottom` is NULL).

We keep track of the `openers_bottom` for each delimiter type (`*`, `_`)
and each length of the closing delimiter run (modulo 3). Initialize this
to `stack_bottom`.

Then we repeat the following until we run out of potential closers:

  - Move `current_position` forward in the delimiter stack (if needed)
    until we find the first potential closer with delimiter `*` or `_`.
    (This will be the potential closer closest to the beginning of the
    input – the first one in parse order.)

  - Now, look back in the stack (staying above `stack_bottom` and the
    `openers_bottom` for this delimiter type) for the first matching
    potential opener (“matching” means same delimiter).

  - If one is found:
    
      - Figure out whether we have emphasis or strong emphasis: if both
        closer and opener spans have length \>= 2, we have strong,
        otherwise regular.
    
      - Insert an emph or strong emph node accordingly, after the text
        node corresponding to the opener.
    
      - Remove any delimiters between the opener and closer from the
        delimiter stack.
    
      - Remove 1 (for regular emph) or 2 (for strong emph) delimiters
        from the opening and closing text nodes. If they become empty as
        a result, remove them and remove the corresponding element of
        the delimiter stack. If the closing node is removed, reset
        `current_position` to the next element in the stack.

  - If none is found:
    
      - Set `openers_bottom` to the element before `current_position`.
        (We know that there are no openers for this kind of closer up to
        and including this point, so this puts a lower bound on future
        searches.)
    
      - If the closer at `current_position` is not a potential opener,
        remove it from the delimiter stack (since we know it can’t be a
        closer either).
    
      - Advance `current_position` to the next element in the stack.

After we’re done, we remove all delimiters above `stack_bottom` from the
delimiter stack.
