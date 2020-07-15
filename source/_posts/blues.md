---
title: Blues & Winamp
date: 2020-05-05 01:01:00
updated: 2020-05-05 01:01:00
categories:
  - Memorial
tags:
  - winamp
  - muzika
---

# Blues & Winamp

{% aplayer  "RL Burnside"  "Someday Baby" "https://sky.monkeyshub.space/muziq/RLBurnsideSomedaybabyfeatLyricsBorn.mp3"  "/fragments/images/rlburnside.jpg"  "width:100%" %}

<!--more-->

{% aplayer  "John Lee Hooker & ZZ Top"  "Boom Boom Boom" "https://sky.monkeyshub.space/muziq/JohnLeeHooker&ZZTopBoomboomboom.mp3"  "/fragments/images/jlhooker.jpg"  "width:100%" %}


##  Winamp Player

<small>
**Nekada {% label warning@Tito %} span role="img" aria-label="smjesko"> :tophat: </span> za mp3 span role="img" aria-label="strelica"> ➡</span> {% label success@Dobri Stari Winamp  %}**
</small>

{% htmlTag %}

  <div id="app" style="height: 70vh">
     </div>
     <script src="https://unpkg.com/webamp@1.4.0/built/webamp.bundle.min.js"></script>
     <script>
         const Webamp = window.Webamp;
         new Webamp({
             initialTracks: [{
                 metaData: {
                     artist: "R L Burnside",
                     title: "Someday baby"
                 },
                 url: "https://sky.monkeyshub.space/muziq/RLBurnsideSomedaybabyfeatLyricsBorn.mp3",
             },
             {
                 metaData: {
                     artist: "REMIX",
                     title: "Howlin Wolf"
                 },
                 url: "https://sky.monkeyshub.space/muziq/HowlinWolfREMIX.mp3",
             },
             {
                 metaData: {
                     artist: "22s-20",
                     title: "Devil in me"
                 },
                 url: "https://sky.monkeyshub.space/muziq/2220devil.mp3",
             },
             {
                 metaData: {
                     artist: "The Record Company",
                     title: "4 Days 3 Nights"
                 },
                 url: "https://sky.monkeyshub.space/muziq/4Days3Nights.mp3",
             },
             {
                 metaData: {
                     artist: "R L Burnside",
                     title: "Im Goin With you Babe"
                 },
                 url: "https://sky.monkeyshub.space/muziq/RLBurnsideImGoinWithyouBabe.mp3",
             },
             {
                 metaData: {
                     artist: "R L Burnside",
                     title: "Dont Stop Honey"
                 },
                 url: "https://sky.monkeyshub.space/muziq/RLBurnsideDontStopHoney.mp3",
             },
             {
                 metaData: {
                     artist: "7 Horses",
                     title: "Meth Lab"
                 },
                 url: "https://sky.monkeyshub.space/muziq/7horsemb.mp3",
             },
             {
                 metaData: {
                     artist: "Devil Makes Three",
                     title: "Ten Feet Tall"
                 },
                 url: "https://sky.monkeyshub.space/muziq/DevilMakesThreeTenFeetTall.mp3",
             },
             {
                 metaData: {
                     artist: "GustaveevatsuG",
                     title: "Sea sick Steve"
                 },
                 url: "https://sky.monkeyshub.space/muziq/GustaveevatsuG__seasickSteve.mp3",
             },
             {
                 metaData: {
                     artist: "R L Burnside",
                     title: "The Criminal Inside Of Me"
                 },
                 url: "/muziq/RLBurnside_TheCriminalInsideOfMe.mp3",
             },
             {
                 metaData: {
                     artist: "John Lee Hooker & ZZ Top",
                     title: "Boom boom boom'"
                 },
                 url: "https://sky.monkeyshub.space/muziq/JohnLeeHooker&ZZTopBoomboomboom.mp3",
             }
             ],
         }).renderWhenReady(document.getElementById('app'));
     </script>

   {% endhtmlTag %}

~ <span role="img" aria-label="slusalice">🎧</span> ~

{% note success %}
U Winamp možete učitati svoj playlist ili muziku sa vašeg diska.
U slučaju da ne vidite player omogućite javaskript u browseru i refrešujte stranicu (F5).
{% endnote %}
