---
title: Winamp Tribute
date: 2020-05-05 01:01:00
updated: 2020-07-10 01:01:00
categories:
  - Memorial
tags:
  - winamp
  - muzika
---

# Winamp Tribute

<div class="text-centar">
<svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="webamp" width="100px" height="100px" viewBox="0 0 100 100">
<circle r="50" cx="50" cy="50" fill="#ea8500" />
<path d="M 49.97 8 A 42 42 0 0 0 8 50 A 42 42 0 0 0 50 92 A 42 42 0 0 0 92 50 A 42 42 0 0 0 50 8 A 42 42 0 0 0 49.97 8 z M 78.21 20.51 L 57.98 45.28 L 71.33 45.73 L 23.85 78.43 L 43.82 55.25 L 27.34 52.57 L 78.21 20.51 z " fill="#383838" />
</svg>
<br />
winamp<sup>web</sup>
</div>

<!--more-->

## Kontrolni playeri

### aplayer

{%  aplayer  "Jain"  "Come"  "https://static.monkeyshub.space/soul/beat/come.opus"  "https://static.monkeyshub.space/fragments/sky/0722-1.png"  "width:100%"  %}

### html audio

<div class="text-centar">
{%  htmlTag  %}
  <img src="https://static.monkeyshub.space/fragments/sky/0722-1.png" height="60px"/>
  <audio controls>
    <source src="https://static.monkeyshub.space/soul/beat/makeba.opus" type="audio/ogg; codecs=opus" />
    <source src="https://static.monkeyshub.space/soul/beat/makeba.ogg" type="audio/ogg; codecs=vorbis" />
    <source src="https://static.monkeyshub.space/soul/beat/makeba.mp3" type="audio/mp3" />
    pokretanje nije moguće, tvoj browser ne podržava <code>audio</code> element. download? <a href="https://static.monkeyshub.space/soul/beat/makeba.mp4">preuzmi fajl</a>.
  </audio>
{%  endhtmlTag  %}
</div>

## Winamp Player

Nekada {% label warning@Tito %} <span role="img" aria-label="sesir"> :tophat: </span> za mp3 <span role="img" aria-label="strelica"> ➡ </span> {% label success@Dobri Stari Winamp %}

{% htmlTag %}

<div id="app" style="height:60vh">
  </div>
    <script src="https://unpkg.com/webamp@1.4.0/built/webamp.bundle.min.js"></script>
    <script>
      const Webamp = window.Webamp;
       new Webamp({
        initialTracks: [{
          metaData: {
             artist: "Jain",
              title: "Makeba"
          },
          url: "https://static.monkeyshub.space/soul/beat/makeba.opus",
          },
          {
          metaData: {
            artist: "Jain",
            title: "Come"
            },
          url: "https://static.monkeyshub.space/soul/beat/come.opus",
          },
          {
          metaData: {
            artist: "None",
            title: "Badunkadunk"
          },
          url: "https://static.monkeyshub.space/soul/beat/badunkadunk.opus",
          }
        ],
      }).
    renderWhenReady(document.getElementById('app'));
  </script>

{% endhtmlTag %}

_Ohhh_ <span role="img" aria-label="slusalice">🎧</span> _bebo_.

## Winamp player integracija

**Rezultat** pogledajte na linku:

{% btn  https://static.monkeyshub.space/apps/amp/, Webamp, fa fa-headphones-alt fa-fw fa-lg, webamp %}

### API bez serviranja

Pozovite _remote_ skriptu:

```html
<script src="https://unpkg.com/webamp@1.4.0/built/webamp.bundle.min.js"></script>
```

### Javascript playlist konstrukcija i rendanje

Pod `script` tagom uredite početnu listu i kreirajte element:

```javascript
const Webamp = window.Webamp;
new Webamp({
  initialTracks: [
    {
      metaData: {
        artist: "Nova",
        title: "Drugi Svijet",
      },
      url: "https://sky.monkeyshub.space/muziq/drugi-svijet.mp3",
      duration: 3.25,
    },
  ],
}).renderWhenReady(document.getElementById("app"));
```

### HTML tag

Gdje god želite, unutar html `body` pozovite element po id-u:

```html
<div id="app" style="height: 100vh">
  !-- Webamp will attempt to center itself within this div -->
</div>
```

{%  label info@To je to %}!

{% note success %}

### Info+

U Winamp možete učitati svoj playlist ili muziku sa vašeg diska.
U slučaju da ne vidite player omogućite javaskript u browseru i refrešujte stranicu <kbd>F5</kbd>.
{% endnote %}

## Winamp svg logo

**Brzi tutorijal** <span role="img" aria-label="mastercam">📷</span>

### Krug + diferencijalni zap

<div class="text-centar">
<svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="krug" width="100px" height="100px" viewBox="0 0 100 100">
<circle r="50" cx="50" cy="50" fill="#ea8500" />
</svg>  + <svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="zapOff" width="100px" height="100px" viewBox="0 0 100 100">
<path d="M 49.97 8 A 42 42 0 0 0 8 50 A 42 42 0 0 0 50 92 A 42 42 0 0 0 92 50 A 42 42 0 0 0 50 8 A 42 42 0 0 0 49.97 8 z M 78.21 20.51 L 57.98 45.28 L 71.33 45.73 L 23.85 78.43 L 43.82 55.25 L 27.34 52.57 L 78.21 20.51 z " fill="#383838" />
</svg>
</div>

```xml
<svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="webamp" width="100px" height="100px" viewBox="0 0 100 100">
<circle r="50" cx="50" cy="50" fill="#ea8500" />
<path d="M 49.97 8 A 42 42 0 0 0 8 50 A 42 42 0 0 0 50 92 A 42 42 0 0 0 92 50 A 42 42 0 0 0 50 8 A 42 42 0 0 0 49.97 8 z M 78.21 20.51 L 57.98 45.28 L 71.33 45.73 L 23.85 78.43 L 43.82 55.25 L 27.34 52.57 L 78.21 20.51 z " fill="#383838" />
</svg>
```

**Rezultat:**

<div class="text-centar">
<svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="webamp" width="100px" height="100px" viewBox="0 0 100 100">
<circle r="50" cx="50" cy="50" fill="#ea8500" />
<path d="M 49.97 8 A 42 42 0 0 0 8 50 A 42 42 0 0 0 50 92 A 42 42 0 0 0 92 50 A 42 42 0 0 0 50 8 A 42 42 0 0 0 49.97 8 z M 78.21 20.51 L 57.98 45.28 L 71.33 45.73 L 23.85 78.43 L 43.82 55.25 L 27.34 52.57 L 78.21 20.51 z " fill="#383838" />
</svg>
</div>

### Krug sa vanjskom linijom + zap

<div class="text-centar">
<svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="krug" width="100px" height="100px" viewBox="0 0 100 100">
<circle r="50" cx="50" cy="50" fill="#383838" stroke="#ea8500" stroke-width="6" stroke-opacity="1" transform="translate(3,97) scale(0.94,-0.94)" />
</svg>  + <svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="zap" width="100px" height="100px" viewBox="0 0 100 100">
<path d="M 78,21 57.53,45.38 71.04,45.82 23,78 43.21,55.19 26.53,52.55 Z" fill="#ea8500" />
</svg>
</div>

```xml
<svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="webamp" width="100px" height="100px" viewBox="0 0 100 100">
<circle r="50" cx="50" cy="50" fill="#383838" stroke="#ea8500" stroke-width="6" stroke-opacity="1" transform="translate(3,97) scale(0.94,-0.94)" />
<path d="M 78,21 57.53,45.38 71.04,45.82 23,78 43.21,55.19 26.53,52.55 Z" fill="#ea8500" />
</svg>
```

**Rezultat**:

<div class="text-centar">
<svg version="1.0" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="webamp" width="100px" height="100px" viewBox="0 0 100 100">
<circle r="50" cx="50" cy="50" fill="#383838" stroke="#ea8500" stroke-width="6" stroke-opacity="1" transform="translate(3,97) scale(0.94,-0.94)" />
<path d="M 78,21 57.53,45.38 71.04,45.82 23,78 43.21,55.19 26.53,52.55 Z" fill="#ea8500" />
</svg>
</div>

{% cq %}
読んでくれてありがとう
{% endcq %}

_Yonde kurete arigatō mein drug, und now enJOY Performanse dole bellow, uživaj Leben_ `tschüss!` <span role="img" aria-label="veselo">:joy:</span>

{% youtube  aVevvbFNKiY %}
