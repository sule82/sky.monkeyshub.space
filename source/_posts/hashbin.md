---
title: hashbin
date: 2020-04-16 01:01:00
updated: 2020-05-16 01:01:00
categories:
  - Hibernation
tags:
  - trash
---

# hashbin pasteBIN

`
HashBin je paste bin koji {% label danger@nikada %} ne vidi _svoj_ zaljepljeni (paste) sadržaj.

Šta je "hash" URL-a, i zašto je specifičan? Hash je dio linka koji počinje sa `#` simbolom. Npr:

```html
http://example.ba#hash
```

A evo zašto je specijalan:

Kada agent (npr web browser) traži web resurs od web servera, agent šalje URI prema serveru, ali ne šalje [hash] `#`.

- [Wikipedia](http://en.wikipedia.org/wiki/Fragment_identifier)

Tako da ga možete koristiti za djeljenje zaljepaka bez da im trakeri uđu u trag.

Na linku dole se nalazi hashbin hosted na mom serveru, i slobodni ste ga koristiti pod [Creative Common licenci](https://creativecommons.org/licenses/by-nc-sa/4.0/):

<div class="text-centar">
{% btn https://static.monkeyshub.space/apps/hashbin/, hashbin, fa fa-hashtag fa-fw, #bin %}
</div>
