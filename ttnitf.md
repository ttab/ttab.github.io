---
layout: page
title: TTNITF
---

Innehållet i detta dokument är avsett för programutvecklare och
tekniskt ansvariga hos mottagarna. Det är en beskrivning av de fält
som kan förekomma i TT:s sändformat TTNITF. Strikt tekniskt gäller
beskrivningen för den SGML-version som TT levererar. För alternativa
leveranser i XML fungerar dokumentet som beskrivning av formatet men
inte som strikt teknisk mall eftersom XML och SGML har vissa
skillnader. I GitHub finns en DTD samt dokumentation på svenska och
engelska.
{: .lead }

<a href="https://github.com/ttab/ttnitf" target="_egen" class="btn btn-primary" role="button">Se på GitHub</a>
<a href="https://github.com/ttab/ttnitf/zipball/master" class="btn btn-primary" role="button">Hämta .zip</a>
<a href="https://github.com/ttab/ttnitf/tarball/master" class="btn btn-primary" role="button">Hämta .tar.gz</a>

### Några termer

* SGML = Standard Generalized Markup Language. En internationellt bestämd standard för att definiera text med avseende på innehållet och dess struktur.
* DTD = Document Type Definition. En mall enligt SGML-standarden som beskriver en viss typ av dokument.
* HTML = En mall (DTD) för dokument på internet.
* IPTC = International Press and Telecommunications Council. (http://www.iptc.org/iptc/)
* IIM = Information Interchange Model. Ett ”elektroniskt kuvert” framtaget av IPTC. (http://www.iptc.org/iptc/standocs.htm)
* NITF = News Industry Text Format. En SGML DTD framtagen av IPTC för nyhetsdistribution internationellt. (http://www.nitf.org)
* TTNIFT = TT:s SGML DTD som bygger dels på IIM och NITF och som registrerats hos IPTC med nummer 27. (http://www.tt.se/)
* XML = eXtensible Markup Language. Utvecklat av W3C som en enklare version av SGML med målsättningen att ge ett bättre alternativ till HTML.

Om Ni har frågor om innehållet eller om TT:s sändformat, kontakta
någon av följande personer:

* Johan Lindgren, Systemutvecklare, 060-176815, johan.lindgren@tt.se
* Anna Nordström, IT-chef, anna.nordstrom@tt.se

### Innehåll

[TTNITF.dtd](https://raw.githubusercontent.com/ttab/ttnitf/master/TTNITF.dtd) | DTD för att validera TTNITF-filer i SGML-format.
[TTNITFversion3_4.pdf](https://raw.githubusercontent.com/ttab/ttnitf/master/TTNITF version3_4.pdf) | Svensk beskrivning av TTNITF med förklaring till element, attribut och värdelistor.
[TTNITFversion3_4eng.pdf](https://raw.githubusercontent.com/ttab/ttnitf/master/TTNITFversion3_4eng.pdf) | Engelsk beskrivning av TTNITF med förklaring till element, attribut och värdelistor.
[TTNITF.dtd_3.4.pdf](https://raw.githubusercontent.com/ttab/ttnitf/master/TTNITF.dtd_3.4.pdf) | Ovanstående DTD i pdf-format på svenska.
[TTNITF.dtd_3.4_eng.pdf](https://raw.githubusercontent.com/ttab/ttnitf/master/TTNITF.dtd_3.4_eng.pdf) | Ovanstående DTD i pdf-format på engelska.

### Revisionshistoria

* Version 1:  Utarbetades och publicerades i december 1996.
* Version 2:  Publicerades i november 1997. Registrerad hos IPTC med nummer 27.
* Version 3:  Publicerades 1998.
* Version 3.1:    Publicerades hösten 1999 med bland annat detaljer för nya sporttabeller och pressmeddelanden.
* Version 3.2:    Publicerades juli 2002. Smärre justeringar av regler i DTD. Påfyllning av nya värden i några listor. Den stora nyheten är id-koder för sportserier, matcher och lag.
* Version 3.3: Publicerades december 2002. Tillägg av attribut för EPOST i BYLINE samt några ytterligare attribut inom sportresultat.
* Version 3.4: Publicerades september 2010. Tillägg av FRAGA och SVAR i BRODTEXT samt stöd för EMBARGODATETIME och TAKE i HEAD.
