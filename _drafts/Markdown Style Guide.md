---
layout: post
title: Markdown Style Guide
---

This is a demo of all styled elements in Jekyll Now.

[View the markdown used to create this post](https://raw.githubusercontent.com/barryclark/www.jekyllnow.com/gh-pages/_posts/2014-6-19-Markdown-Style-Guide.md).

This is a paragraph, it's surrounded by whitespace. Next up are some headers, they're heavily influenced by GitHub's markdown style.

## Header 2 (H1 is reserved for post titles)##

### Header 3

#### Header 4

A link to [Jekyll Now](http://github.com/barryclark/jekyll-now/). A big ass literal link <http://github.com/barryclark/jekyll-now/>

An image, located within /images

![an image alt text](Blog/assets/img/jekyll-logo.png "an image title")

* A bulletted list
- alternative syntax 1
+ alternative syntax 2
  - an indented list item

1. An
2. ordered
3. list

Inline markup styles:

- _italics_
- **bold**
- `code()`

| Tables   |      Are      |  Cool |
|----------|:-------------:|------:|
| col 1 is |  left-aligned | $1600 |
| col 2 is |    centered   |   $12 |
| col 3 is | right-aligned |    $1 |

|  |  |  |  |  | 23 | 7 |
|:--------:|-----|----------|-----|---|----|----|
|  |  |  |  | 7 | 0 | 1 |
| 23=3*7+2 | <=> | 2=23-3*7 | ... | 2 | 1 | -3 |
| 7=3*2+1 | <=> | 1=7-3*2 | ... | 1 |  |  |
| 2=2*1+0 | <=> | 0=2-2*1 | ... |  |  |  |

|  	|  	|  	|  	|  	| 23 	| 7 	|
|:--------:	|-----	|----------	|-----	|---	|----	|----	|
|  	|  	|  	|  	| 7 	| 0 	| 1 	|
| 23=3*7+2 	| <=> 	| 2=23-3*7 	| ... 	| 2 	| 1 	| -3 	|
| 7=3*2+1 	| <=> 	| 1=7-3*2 	| ... 	| 1 	|  	|  	|
| 2=2*1+0 	| <=> 	| 0=2-2*1 	| ... 	|  	|  	|  	|

> Blockquote
>> Nested Blockquote

Syntax highlighting can be used by wrapping your code in a liquid tag like so:

{{ "{% highlight javascript " }}%}
/* Some pointless Javascript */
var rawr = ["r", "a", "w", "r"];
{{ "{% endhighlight " }}%}

creates...

{% highlight javascript %}
/* Some pointless Javascript */
var rawr = ["r", "a", "w", "r"];
{% endhighlight %}

Use two trailing spaces
on the right
to create linebreak tags

Finally, horizontal lines

----
****
