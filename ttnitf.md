---
layout: page
title: TTNITF
---

<div class="lead">Innehållet i detta dokument är avsett för programutvecklare och tekniskt ansvariga hos mottagarna. Det är en beskrivning av de fält som kan förekomma i TT:s sändformat TTNITF. Strikt tekniskt gäller beskrivningen för den SGML-version som TT levererar. För alternativa leveranser i XML fungerar dokumentet som beskrivning av formatet men inte som strikt teknisk mall eftersom XML och SGML har vissa skillnader. I GitHub finns en DTD samt dokumentation på svenska och engelska.</div>

<a href="https://github.com/ttab/ttnitf" target="_egen" class="btn btn-primary" role="button">Se på GitHub</a>
<a href="https://github.com/ttab/ttnitf/zipball/master" class="btn btn-primary" role="button">Hämta .zip</a>
<a href="https://github.com/ttab/ttnitf/tarball/master" class="btn btn-primary" role="button">Hämta .tar.gz</a>

<br/><br/>
<h4>Några termer:<h4>
<ul>
<li>SGML = Standard Generalized Markup Language. En internationellt bestämd standard för att definiera text med avseende på innehållet och dess struktur.</li>
<li>DTD = Document Type Definition. En mall enligt SGML-standarden som beskriver en viss typ av dokument.</li>
<li>HTML = En mall (DTD) för dokument på internet.</li>
<li>IPTC = International Press and Telecommunications Council. (http://www.iptc.org/iptc/)</li>
<li>IIM = Information Interchange Model. Ett ”elektroniskt kuvert” framtaget av IPTC. (http://www.iptc.org/iptc/standocs.htm)</li>
<li>NITF = News Industry Text Format. En SGML DTD framtagen av IPTC för nyhetsdistribution internationellt. (http://www.nitf.org)</li>
<li>TTNIFT = TT:s SGML DTD som bygger dels på IIM och NITF och som registrerats hos IPTC med nummer 27. (http://www.tt.se/)</li>
<li>XML = eXtensible Markup Language. Utvecklat av W3C som en enklare version av SGML med målsättningen att ge ett bättre alternativ till HTML.</li>
</ul>
<br/><br/>
<br/><br/>


<div>Om Ni har frågor om innehållet eller om TT:s sändformat, kontakta någon av följande personer:<br/><br/>
Johan Lindgren, Systemutvecklare, 060-176815, johan.lindgren@tt.se<br/><br/>  
Anna Nordström,    IT-chef,        anna.nordstrom@tt.se<br/><br/>
</div>


    
    <h4>Innehåll </h4>
    <table>
    <tr><td>TTNITF.dtd</td><td>DTD för att validera TTNITF-filer i SGML-format.</td></tr>
    <tr><td>TTNITF version3_4.pdf</td><td>Svensk beskrivning av TTNITF med förklaring till element, attribut och värdelistor.</td></tr>
    <tr><td>TTNITFversion3_4eng.pdf</td><td>Engelsk beskrivning av TTNITF med förklaring till element, attribut och värdelistor.</td></tr>
    <tr><td>TTNITF.dtd_3.4.pdf</td><td>Ovanstående DTD i pdf-format på svenska.</td></tr>
    <tr><td>TTNITF.dtd_3.4_eng.pdf</td><td>Ovanstående DTD i pdf-format på engelska.</td></tr>
</table>    
    
    
    <h4>Revisionshistoria.</h4>
<ul>
<li>Version 1:  Utarbetades och publicerades i december 1996.</li>
<li>Version 2:  Publicerades i november 1997. Registrerad hos IPTC med nummer 27.</li>
<li>Version 3:  Publicerades 1998.</li>
<li>Version 3.1:    Publicerades hösten 1999 med bland annat detaljer för nya sporttabeller och pressmeddelanden.</li>
<li>Version 3.2:    Publicerades juli 2002. Smärre justeringar av regler i DTD. Påfyllning av nya värden i några listor. Den stora nyheten är id-koder för sportserier, matcher och lag.</li>
<li>Version 3.3: Publicerades december 2002. Tillägg av attribut för EPOST i BYLINE samt några ytterligare attribut inom sportresultat.</li>
<li>Version 3.4: Publicerades september 2010. Tillägg av FRAGA och SVAR i BRODTEXT samt stöd för EMBARGODATETIME och TAKE i HEAD.</li>

 </ul>

