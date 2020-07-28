---
title: CLI biseri
date: 2020-05-17 18:43:00
updated: 2020-05-29 22:07:00
categories:
  - Tutorials
tags:
  - konzola
  - terminal
  - komande
---

# Linux terminal / konzolne komande

{%  codeblock lang:bash  "cli" %}
$ ls -hal --my-awesome-commands
{%  endcodeblock  %}

{%  note success  %}
2020-05-29 --zadnji _update_
{%  endnote %}

<!--more-->

##  sudo

{% codeblock lang:bash [ponavlja zadnju liniju kao sudo] %}
sudo !!
{% endcodeblock %}

##  http server

{% codeblock lang:bash [python 2] %}
python -m SimpleHTTPServer
{% endcodeblock %}

{% codeblock lang:sh [python 3] %}
python -m http.server
{% endcodeblock %}

##  mount

{% codeblock lang:bash [tabelarni prikaz]  %}
column -t /proc/mounts
{% endcodeblock %}

{% codeblock lang:sh [tabelarni prikaz]  %}
mount | column -t
{% endcodeblock %}

##  wget

{% codeblock lang:bash [full sajt dowload + konverzija linkova]  %}
wget -mkEpnp websajt.ba   # robots=off -U mozilla
{% endcodeblock %}

{% codeblock lang:bash [extract tarball bez lokalnog cuvanja]  %}
wget -qO - "http://www.tarball.ba/tarball.gz" | tar zxvf -
{% endcodeblock %}

##  ping

{% codeblock lang:bash  [ping svakih 60s sa audio dojavom kada adresa bude pingabilna]  %}
ping -i 60 -a IP_address
{% endcodeblock %}

##  procesi

{% codeblock lang:bash [top 10 procesa sortiranih po crpljenju memorije]  %}
ps aux --sort -rss | head
{% endcodeblock %}

{% codeblock lang:bash [top 10 procesa sortiranih po crpljenju memorije]  %}
ps axo %mem,pid,euser,cmd | sort -nr | head -n 10
{% endcodeblock %}

{% codeblock lang:bash [grep procesa za minus sam grep]  %}
ps aux | grep [p]rocess-name
{% endcodeblock %}

{% codeblock lang:bash [trazenje aktivnog procesa bez kesiranja pretrage]  %}
ps -ef | awk '/process-name/ && !/awk/ {print}'   # print$2 lista samo pidove
{% endcodeblock %}

##  remove & delete

{% codeblock lang:bash [brisanje svih fajlova u folderu osim !(x.y)]  %}
rm -f !(jedinifajlkojinecebitiobrisan.txt)
{% endcodeblock %}

{% codeblock lang:bash [brisanje svih fajlova u folderu osim]  %}
find . ! -name <FILENAME> -delete
{% endcodeblock %}

{% codeblock lang:bash [brisanje svih fajlova u folderu osim !(x.y)]  %}
rm -f rm !(*.txt|*.html|*.json|*.ico)
{% endcodeblock %}


## skriptanje

{% codeblock lang:bash [snimanje prethodne komande u skriptu (!!:q za komentiranje linija)]  %}
echo "!!" > foo.sh
{% endcodeblock %}

{% codeblock lang:bash [snimanje prethodne komande u skriptu]  %}
history -1 | sed -e 's/^\ [0-9]*\ *//g' > foo.sh  
{% endcodeblock %}

##  ls & ss

{% codeblock lang:bash  [prikaz aplikacija koje koriste internet trenutno]  %}
ss -p
{% endcodeblock %}

{% codeblock lang:bash  [prikaz aplikacija koje koriste internet trenutno]  %}
lsof -P -i -n
{% endcodeblock %}

{% codeblock lang:bash  [prikaz samo naziva aplikacija koje koriste internet trenutno]  %}
lsof -P -i -n | cut -f 1 -d " "| uniq | tail -n +2
{% endcodeblock %}

{% codeblock lang:bash  [tree pregled subfoldera]  %}
ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/ /' -e 's/-/|/'
{% endcodeblock %}

{% codeblock lang:bash  [ko koristi ovaj port?]  %}
lsof -i tcp:8080
{% endcodeblock %}

##  kill

{% codeblock lang:bash  [kill fajla / skripte / procesa koji ti ide na zivce]  %}
fuser -k filename
{% endcodeblock %}

{% codeblock lang:bash  [stop & suspend]  %}
killall -STOP -m firefox
killall -CONT -m firefox  #rezultira sa Zero cpu
{% endcodeblock %}

##  find

{% codeblock lang:bash  [pronalazi duple fajlove, prvo prema velicini, zatim po md5sum]  %}
find -not -empty -type f -printf "%-30s'\t\"%h/%f\"\n" | sort -rn -t$'\t' | uniq -w30 -D | cut -f 2 -d $'\t' | xargs md5sum | sort | uniq -w32 --all-repeated=separate
{% endcodeblock %}

{% codeblock lang:bash  [brise prazne foldere unutar radnog]  %}
find . -type d -empty -delete
{% endcodeblock %}

{% codeblock lang:bash  [pronadji fajlove koji su izmjenjeni u zadnjih 60min]  %}
find / -mmin 60 -type f
{% endcodeblock %}

##  mkdir & cd

{% codeblock lang:bash  [mkdir & cd u njega / portable]  %}
mkdir /home/banana/doc/monkey && cd $_
{% endcodeblock %}

{% codeblock lang:bash  [sa nizom subfoldera + 4 foldera u zadnjem :)]  %}
mkdir -p duga/direktorij/putanja/{abc,cde,defc,efca}
{% endcodeblock %}

{% codeblock lang:bash  [wordpress style]  %}
mkdir -p uploads/{2017..2020}/{01..12}/{1..31}
{% endcodeblock %}

##  Održavanje konzolne

{% codeblock lang:bash  [ocisti ili resetuj]  %}
clear #ctrl-l
reset   
{% endcodeblock %}

{% codeblock lang:bash  [izbrisi historiju]  %}
history -d [broj linije]  
<kbd>backspace</kbd>
{% endcodeblock %}


##  IP API (ipify.org)

{% codeblock lang:bash  [bash]  %}
ip=$(curl -s https://api.ipify.org)
echo "Javna IP adresa: $ip"
{% endcodeblock %}

{% codeblock lang:python  [python]  %}
ip=$(curl -s https://api.ipify.org)
echo "Javna IP adresa: $ip"
{% endcodeblock %}

{% codeblock lang:bash  [bash]  %}
from requests import get

ip = get('https://api.ipify.org').text
print('Javna IP adresa: {}'.format(ip))
{% endcodeblock %}

{% codeblock lang:javascript  [javascript]  %}
<script type="application/javascript">
  function getIP(json) {
    document.write("moja javna IP adresa je: ", json.ip);
</script>
<script type="application/javascript" src="https://api.ipify.org?format=jsonp&callback=getIP"></script>
{% endcodeblock %}

{% codeblock lang:javascript  [node.js]  %}
var http = require('http');
http.get({'host': 'api.ipify.org', 'port': 80, 'path': '/'}, function(resp) {
  resp.on('data', function(ip) {
    console.log("My public IP address is: " + ip);
  });
});
{% endcodeblock %}


U ovom članku možete pronaći više IP, kao i drugih korisnih servisa: {% post_link konzolni-servisi %}
