---
title: Notes & Hibernation
date: 2020-04-05 01:01:00
updated: 2020-05-16 01:01:00
categories:
  - Hibernation
tags:
  - trash
---

# Zapiši i Zaboravi

<!-- more -->

## Emojis - very special few

_Uvijek vidljivi_

btw. da ispoštujemo pravila, ikone i smajliće je potrebno upakovati u `<span>` zadati `role` i 'label',

```html
<span role="img" aria-label="Godzilla">🐕</span>
```

copy-paste:

```bash
🐟 🐦 🐕 🐈

❤ 👪 👓 ⛄ 🌜 🌍 🌎 🌏 ⭐ ☀ ⛅ ☁ ⚡ ☔ ❄ ⛄

💻 📷 📹 💿 ☎ 📟 📺 📻 🎓 🔊 🔉 🔈 🔇 ⏳ ⌛ ⌚ 🔓 🔒 🔍 💣 💰 💳

✉ 📥 📤 📫 📪 📬 📭 📦 📊 📈 📉 📋 ✂ ✒ ✏ 📚

🎧 🎬 🎮 🃏 🀄 ⚽ ⚾ ⛳ 🏆 🏂 🏊 🏄 🍸

🎭 🏠 ⛪ ⛺ 🏭 ⛲ ⛵ ⚓ ✈ 🚇 🚍 🚘 🚔 🚑 🚲 ⚠ ⛽ ♨

➡ ⬅ ⬆ ⬇ ↗ ↖ ↘ ↙ ↔ ↕ ◀ ▶ ↩ ↪ ℹ ⏪ ⏩ ⤵ ⤴

♻ 🚹 🚺 🚼 🅿 ♿ 🚭 Ⓜ ㊙ ㊗ ⛔ ✳ ❇ ✴ 🅰 🅱 🆎 🅾

⭕ 💹 © ® ™ ‼ ⁉ ❗ ❓ 🔃 ✖ ♠ ♥ ♣ ♦ ✔ ☑ 〰 〽 ◼ ◻ ◾ ◽ ▪ ▫ ⚫ ⚪ ⬜ ⬛

🕛 🕧 🕐 🕜 🕑 🕝 🕒 🕞 🕓 🕟 🕔 🕠 🕕 🕖 🕗 🕘 🕙 🕚 🕡 🕢 🕣 🕤 🕥 🕦
```

```
     _
 ___| | ___   _
/ __| |/ / | | |
\__ \   <| |_| |
|___/_|\_\\__, |o
          |___/
               _                 _       _
 _____ ___ ___| |_ ___ _ _ ___  | |_ _ _| |_
|     | . |   | '_| -_| | |_ -| |   | | | . |
|_|_|_|___|_|_|_,_|___|_  |___|o|_|_|___|___|
                      |___|
```

### HTML Entities

| rezultat | opis                   | entity naziv | entity broj |
| :------: | :--------------------- | -----------: | ----------: |
|          | non-breaking space     |     `&nbsp;` |    `&#160;` |
|    <     | manje                  |       `&lt;` |     `&#60;` |
|    >     | veće                   |       `&gt;` |     `&#62;` |
|    &     | ampersand              |      `&amp;` |     `&#38;` |
|    "     | dupla kvota            |     `&quot;` |     `&#34;` |
|    '     | kvota (apostrof)       |     `&apos;` |     `&#39;` |
|    ¢     | cent                   |     `&cent;` |    `&#162;` |
|    £     | funta                  |    `&pound;` |    `&#163;` |
|    ¥     | jen                    |      `&yen;` |    `&#165`; |
|    €     | euro                   |     `&euro;` |   `&#8364;` |
|    ©     | copyright              |     `&copy;` |    `&#169;` |
|    ®     | registrovani trademark |      `&reg;` |    `&#174;` |

### Paper Arts

Arhive knjiga:

{% btn https://shou.monkeyshub.space/hashbin/#IwOgUAFgLlAODOAuA9M+BjCB7ANgQwCcQBzLLYnAUxHSwFtkwAmcaOJVPdPAE0roCW6EIPQEs8LADMoNeowDMrGAhTIIBbgGsQ8TdQ2MALMrhqMAgHZ8t8GpSyWQBeIwCsp1alg4ArnYAjCDkAgXg7S0ooRgA2Tw5kAHdkkB4sPAArECwCYkYAdni1ZMT7Bxw5BjBqgGI6gAJG+vgorAA3ASa6muq2L2QePCg8CF8AkAEsRj6EgC9KSyw07NzGMCA===, Biblioteka, fa fa-kiwi fa-fw fa-lg, Biblioteka %}

- E knjiga u morzeovu azbuku (MP3s/OGGs)

```bash
ebook2cw [-w wpm] [-f freq] [-R risetime] [-F falltime]
         [-s samplerate] [-b mp3bitrate] [-q mp3quality] [-p]
         [-c chapter-separator] [-o outfile-name] [-Q minutes]
         [-a author] [-t title] [-k comment] [-y year]
         [-u] [-S ISO|UTF] [-n] [-e eff.wpm] [-W space]
         [-N snr] [-B filter bandwidth] [-C filter center]
         [-T 0..2] [-g filename] [-l words] [-d seconds]
         [infile]
```

```bash
wkhtmltopdf
```

## Image & Video

```bash
imgp  -prew
```

```bash
ffmpeg  [global_options] {[input_file_options] -i input_url} ...
                         {[output_files_options] output_url} ...
  #opis:
   _______              ______________
  |       |            |              |
  | input |  demuxer   | encoded data |   decoder
  | file  | ---------> | packets      | -----+
  |_______|            |______________|      |
                                             v
                                         _________
                                        |         |
                                        | decoded |
                                        | frames  |
                                        |_________|
   ________             ______________       |
  |        |           |              |      |
  | output | <-------- | encoded data | <----+
  | file   |   muxer   | packets      |   encoder
  |________|           |______________|

  #jednostavni copy stream:
               +-------+            +--------------+          +--------+
               | input |  demuxer   | encoded data |  muxer   | output |
               | file  | ---------> | packets      | -------> | file   |
               +-------+            +--------------+          +--------+
```

## Trackers on the line

```bash
facebook-cli
    #COMMANDS
    api      - Make a direct Facebook API request
    config   - Save your Facebook API credentials
    feed     - List posts on your timeline
    help     - Shows a list of commands or help for one command
    likes    - List pages you have 'Liked'
    links    - Some useful URLs
    login    - Request Facebook permissions and receive an API access token
    logout   - Deauthorize your access token
    me       - Show your profile information
    photos   - List photos you have uploaded
    photosof - List photos you are tagged in
    videos   - List videos you have uploaded
    videosof - List videos you are tagged in
facebook-cli config --appid=557576381591271 --appsecret=5b0de472815c3759414030a93c796fdd
```

```bash
pacman -S youtube-dl && npm i -g youtube-dl-interactive
  # pazi na PATH :)
```

# Custom ROM [Android]

#### Instalacija Clean k'o Suza u 10 Koraka

1.  Ne radi nish
2.  Flash 10.0.13.0 (ovo je za moj daisy)
3.  Boot u TWRP
4.  Wipe podataka
5.  Instalacija ROM-a
6.  Instalacija TWRP
7.  Promijeni slot (A/B)
8.  Reboot u Recovery
9.  Instalacija Gapps-a
10. Instalacija Magisk-a (opc.)
11. Reboot u system

I nij.. {%  label info@[...stej tjun'd for treš...] %}
