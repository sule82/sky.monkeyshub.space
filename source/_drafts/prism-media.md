---
title: Prism media
date:
updated:
draft: true
categories:
  - Tutorial
tags:
  - prism
  - media
---

[TOC]

# prism-media :rocket:

## Usage

```javascript
// This example will demux and decode an Opus-containing OGG file, and then write it to a file.
const prism = require('prism-media');
const fs = require('fs');

fs.createReadStream('./audio.ogg')
  .pipe(new prism.opus.OggDemuxer())
  .pipe(new prism.opus.Decoder({ rate: 48000, channels: 2, frameSize: 960 }))
  .pipe(fs.createWriteStream('./audio.pcm'));
```

## Dependencies

### Opus

- @discordjs/opus
- node-opus
- opusscript

#### @discordjs/opus

```javascript
const { OpusEncoder } = require('@discordjs/opus');

// Create the encoder.
// Specify 48kHz sampling rate and 2 channel size.
const encoder = new OpusEncoder(48000, 2);

// Encode and decode.
const encoded = encoder.encode(buffer, 48000 / 100);
const decoded = encoder.decode(encoded, 48000 / 100);
```

### FFmpeg

- ffmpeg-static
- ffmpeg from a normal installation

[Docs](htthttps://hydrabolt.me/prism-mediap:// "Docs")

### docma :books:

> Docma is a powerful tool to easily generate beautiful HTML documentation from Javascript (JSDoc), Markdown and HTML files. Docma is a powerful tool to easily generate beautiful HTML documentation from Javascript (JSDoc), Markdown and HTML files.

[Sajt](https://onury.io/docma/ "Sajt")


`npm i docma -D`
