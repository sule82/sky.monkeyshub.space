---
title: HEXO Tags
date: 2020-04-05 01:01:00
updated: 2020-05-16 01:01:00
categories:
  - Hibernation
tags:
  - trash
---


# hexo tag plugins

```
{% hexotag %}
{% endhexotag %}
```
<!--more-->

##  blockquote

```
{% blockquote [author[, source]] [link] [source_link_title] %}
content
{% endblockquote %}
```


##  codeblock

alias: code

```
{% codeblock [title] [lang:language] [url] [link text] [additional options] %}
code snippet
{% endcodeblock %}
```

Dodatne opcije specifiramo u `option:value` formatu:

| Extra Opcija | Opis | Predpodešeno  |
| --- | --- | --: |
| `line_number` | prikazuje linijski broj | `true` |
| `highlight` | omogućuje highlight koda | `true` |
| `first_line` | specificira prvi linijski broj | `1` |
| `mark` | linijski highlight specifičnih linija, npr: `mark:1,4-7,10` markira 1, 4 do 7 i 10 liniju |  bez  |
| `wrap` | code block u [`<table>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table) | `true` |


##  pullquote

```
{% pullquote [class] %}
content
{% endpullquote %}
```


##  jsfiddle

```
{% jsfiddle shorttag [tabs] [skin] [width] [height] %}
```


##  iframe

```
{% iframe url [width] [height] %}
```


##  img

```
{% img [class names] /path/to/image [width] [height] '"title text" "alt text"' %}
```


## link

`target="_blank"`:

```
{% link text url [external] [title] %}
```

##  include_code

```
{% include_code [title] [lang:language] [from:line] [to:line] path/to/file %}
```

##  video

###  youtube

```
{% youtube video_id %}
```

###  vimeo

```
{% vimeo video_id [width] [height] %}
```

###  video source link

```
{% video url %}
```


##  post_link

```
{% post_link filename [title] [escape] %}
```

{% label info@ne treba path %} - filename = filename.md



## assets_link

```
{% asset_path filename %}
{% asset_img filename [title] %}
{% asset_link filename [title] [escape] %}
```


## raw

```
{% raw %}
content
{% endraw %}
```

# injected

{% label danger@next.js  %} `_config.yml`


##  note

```
{% note [class] [no-icon] %}
content
{% endnote %}
```

class: default | primary | success | info | warning | danger


##  tabs

```
{% tabs Unique name, [index] %}
<!-- tab [Tab caption] [@icon] -->
content
<!-- endtab -->
{% endtabs %}
```

##  pdf

```
{% pdf url [height] %}
```

##  mermaid

```
{% mermaid type %}
{% endmermaid %}
```

{% btn https://github.com/mermaid-js/mermaid, Info, fa-fa-code-branch fa-fw, github %}

{% post_link mermaid %}

##  label

```
{% label [class]@Text %}
```

## button

alias: btn

```
{% button url, text, icon [class], [title] %}
```


##  caniuse

alias: can

```
{% caniuse feature @ periods %}
```

{%  can fetch %}


##  grouppicture

alias: gp

```
{% grouppicture [group]-[layout] %}{% endgrouppicture %}
```


##  linkgrid

alias: lg

```
{% linkgrid [image] [delimiter] [comment] %}{% endlinkgrid %}
```
