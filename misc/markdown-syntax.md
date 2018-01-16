---
layout: page
title: Note for Markdown Syntax
comments: no
---

Markdown rocks!
===========


This cheatsheet is specifically [Stackedit](https://stackedit.io) and [Jekyll](https://jekyllrb.com) version of Github-flavored Markdown.

Github also provides some references for `GitHub Flavored Markdown` syntax:

- [Markdown Basics](https://help.github.com/articles/markdown-basics)
- [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown)
- [Writing on GitHub](https://help.github.com/articles/writing-on-github)




--------

## 1. Table of Contents
- In [StackEdit](https://stackedit.io), you can insert a table of contents using the marker `[TOC]`, then StackEdit will automaticly generate it for you.

```
[TOC]
```

- In common Markdown page, we can set the anchor using HTML syntax for quickly jumping in the page.

```
//set anchor in the page
<span id="jump-here"></span>
...
...
...
//set Jump link
[Jump to Headers](#jump-here)
```

[Jump to Headers](#jump-here)

--------

<span id="jump-here"></span>

## 2. Headers

```

# H1

## H2

### H3

#### H4

##### H5

###### H6

Alternatively, for H1 and H2, an underline-ish style:

Alt-H1
======

Alt-H2
------
```

# H1

## H2

### H3

#### H4

##### H5

###### H6

Alt-H1
======

Alt-H2
------

--------

## 3. Emphasis

```
Italics, with *asterisks* or _underscores_.

Bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~
```

Italics, with *asterisks* or _underscores_.

Bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

--------

## 4. Lists

> **Manually line breaks: End a line with 2 or more spaces.**


```
1. First ordered list item
2. Another item
    * Unordered sub-list.(indented with 'tab')
1. Actual numbers don't matter, just that it's a number
    1. Ordered sub-list
4. And another item.
    * Unordered list can use asterisks
    - Or minuses
    + Or pluses
```

1. First ordered list item
2. Another item
    * Unordered sub-list.(indented with 'tab')
1. Actual numbers don't matter, just that it's a number
    1. Ordered sub-list
4. And another item.
    * Unordered list can use asterisks
    - Or minuses
    + Or pluses

--------

## 5. Links

There are two ways to create links.

- **Inline style:**

```
An [example](http://url.com/somewhere)
```

An [example](https://stackedit.io)

- **Reference style:**

```
An [example][id]
...
...
...
```
`[id]: http://url.com/somewhere`

```
An [example][]
...
...
...
```
`[example]: http://url.com/somewhere`


An [example][id]

[id]: http://url.com/somewhere "Title"

An [example][]

[example]: http://url.com/somewhere "Title"

----------

## 6. Images

- **Inline-style:**

```
![Example](path/to/image.jpg "Title")
```

![Example](/avatar.jpg)

> **Note**:
- "Title" is optional here.
- Don't forget 'http://' or 'https://' when linking to a web image.

----------

## 7. Code and Syntax Highlighting

```
Inline `code` has `back-ticks around` it.
```

- Inline `code` has `back-ticks around` it.


>Use <code>...</code> to handle some problems that `...` couldn't.


- Blocks of code are either fenced by lines with three back-ticks <code>```</code>, or are indented with four spaces. I recommend only using the fenced code blocks -- they're easier and only they support syntax highlighting -- and don't forget to leave a blank line before the block.


- In [jekyll](http://jekyllrb.com/), we highlight the code with [pygments](http://pygments.org/), the syntax of which is like [here](http://jekyllrb.com/docs/templates/)

```
{********}
def foo
  puts 'foo'
end
{********}
```

>We type in `% highlight ruby %` in the first `********`, `% endhighlight %` in the second `********`

{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}

----------

## 8. Tables

```
Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

The outer pipes (|) are optional, and you don't need to make the raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
```

Colons can be used to align columns.

| Tables        | Are           | Cool |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

The outer pipes (|) are optional, and you don't need to make the raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3


----------

## 9. Blockquotes

```
> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.
```

**Note:**

```
In StackEdit, the code in ''' ''' blocks will be automaticly highlighted.
But in kramdown environment, we use ``` ``` instead to write the blockquote and code block.
```


> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.

----------

## 10. Inline HTML

You can also use raw HTML in your Markdown, and it'll mostly work pretty well.

{% highlight html %}
<dl>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>
{% endhighlight %}

<dl>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>
---------

## 11. Horizontal Rule

```
Three or more...

---

Hyphens

***

Asterisks

___

Underscores
```

Three or more...

---

Hyphens

***

Asterisks

___

Underscores


----------

## 12. MathJax

Thanks to [MathJax](http://www.mathjax.org), we can use `$...$`(for inline) and `$$...$$`(for block) to render Math Symbols, like this: $A$, and Equations.


$$
b(s) = \int_{-\infty}^{\infty}p(s-t)x(t)dt
$$

>You might want to add MathJax feature to your [Github Pages](https://help.github.com/categories/20/articles) based on [jekyll](http://jekyllrb.com/), thus this [manual](http://docs.mathjax.org/en/latest/start.html) will be helpful. **Note:** As the manual says, the `$...$` in-line delimiters are **not** used by default, you have to turn on this feature manually.
