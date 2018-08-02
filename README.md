# CETEIcean Examples
## TEI [Technical Council Working documents](http://teic.github.io/TCW). Sources in <http://github.com/TEIC/Documentation>. 
Because of the way CETEIcean and GitHub Pages work, changes to documents in the repo show up very quickly. Very easy to make updates (e.g. during the course of a release).

## Digital Latin Library [viewer](http://digitallatin.github.io/viewer/calpurnius.html)
Same deal, much more complex document. The apparatus is generated from inline TEI `<app>` tags, and likewise the apparatus "widgets" in the margin are built with Javascript and CSS.
```
O.  Hoc potius, frater Corydon, nemus, antra petamus
    ista patris Fauni,
```
translation: "O(rnytus): "Let us rather, brother Corydon, seek this wood, those groves of father Faunus,..."

Apparatus:

9 ista] NG **Glaeser sqq.** antra PV **edd. ante Glaeser**

translation: the manuscripts "N" and "G", and editions following Glaeser's put "antra" in line 8 and "ista" in line 9; "P", "V", and editions before Glaeser's do the reverse.

```xml
<l n="8"><label type="speaker">
    <app>
      <lem xml:id="lem1.7-label">O.</lem>
      <rdg wit="#N" xml:id="rdg1.7-omission" ana="#subtractive"/>
      <witDetail wit="#N" target="rdg1.7-omission" type="correction-original"/>
    </app>
  </label>Hoc potius, frater <app>
    <lem wit="#χ #ε #δ #ρ2" source="#edd.">Corydon</lem>
    <rdg wit="#N #G #β #ν #κ #λ #γ #μ #π #ρ1 #φ #α #η #r" source="#Barth1613"
      ana="#orthographical">coridon</rdg>
    <rdg wit="#P" ana="#orthographical">corridon</rdg>
  </app>, <app>
    <lem>nemus</lem>
    <rdg wit="#G1" ana="#morphological"><add place="margin">nemora</add></rdg>
  </app>, <app type="transposition">
    <lem wit="#N #G" xml:id="l8a1lem1" require="#l9a1lem1">antra</lem>
    <note><ref target="#Glaeser">Glaeser</ref> sqq.</note>
    <rdg wit="#P #V" xml:id="l8a1rdg1" require="#l9a1rdg1" ana="#ordinal">ista</rdg>
    <note target="#l8a1rdg1">edd. ante <ref target="#Glaeser">Glaeser</ref></note>
  </app> petamus</l>
<l n="9"><app type="transposition">
    <lem wit="#N #G" xml:id="l9a1lem1" require="#l8a1lem1">ista</lem>
    <note><ref target="#Glaeser">Glaeser</ref> sqq.</note>
    <rdg wit="#P #V" xml:id="l9a1rdg1" require="#l8a1rdg1" ana="#ordinal">antra</rdg>
    <note target="#l9a1rdg1">edd. ante <ref target="#Glaeser">Glaeser</ref></note>
  </app> patris <persName ref="#Faunus">Fauni</persName>, ...
  </app></l>
```

So if you change antra to ista in line 8, ista has to change to antra in line 9.

## Papyri.info
See <http://papyri.info/ddbdp/p.fay;;110>. This uses the EpiDoc stylesheets to convert the XML source to HTML. This is a complex piece of software, over 11,000 lines of XSLT. Could CETEIcean replicate it? Yes, though it's a bit painful. We made some (what I now regard as) poor life choices with the source XML. E.g. 
```xml
<hi rend="diaeresis">ἰ</hi>δίωι
``` 
has to become "ϊδίωι papyrus" in the papyrus. So we have to grab the text surrounding the `<hi>` to make the apparatus entry. Word-breaking `<lb>`'s (which need to be hyphenated) have whitespace before them, which is stupid. 

There are also bits that aren't stupid, but are nonetheless quite hard. `<gap reason="lost">` is represented as something like "[..]" (with square brackets). `<supplied reason="lost">` gets square brackets too. But when you have something like:
```xml
<gap reason="lost" quantity="3" unit="character"/><supplied reason="lost"> κα</supplied><unclear>ὶ</unclear>
```
You don't want [...][κα]ὶ̣, but rather [... κα]ὶ̣. The example file does some of this stuff by post-processing the markup, but the majority is done with CETEIcean behaviors.