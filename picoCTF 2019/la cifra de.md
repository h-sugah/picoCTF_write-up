# la cifra de
Author: Alex Fulton/Daniel Tunitis  

### URL
https://play.picoctf.org/practice/challenge/3  

### Description
I found this cipher in an old book. Can you figure out what it says? Connect with nc jupiter.challenges.picoctf.org 32411.  

<br>
<br>

## 解答の過程
問題文で指示されているサイトにncコマンドでアクセスすると、何かの文章のようなテキストが出力されます。  

```
> nc jupiter.challenges.picoctf.org 32411
Encrypted message:
Ne iy nytkwpsznyg nth it mtsztcy vjzprj zfzjy rkhpibj nrkitt ltc tnnygy ysee itd tte cxjltk

Ifrosr tnj noawde uk siyyzre, yse Bnretèwp Cousex mls hjpn xjtnbjytki xatd eisjd

Iz bls lfwskqj azycihzeej yz Brftsk ip Volpnèxj ls oy hay tcimnyarqj dkxnrogpd os 1553 my Mnzvgs Mazytszf Merqlsu ny hox moup Wa inqrg ipl. Ynr. Gotgat Gltzndtg Gplrfdo

Ltc tnj tmvqpmkseaznzn uk ehox nivmpr g ylbrj ts ltcmki my yqtdosr tnj wocjc hgqq ol fy oxitngwj arusahje fuw ln guaaxjytrd catizm tzxbkw zf vqlckx hizm ceyupcz yz tnj fpvjc hgqqpohzCZK{m311a50_0x_a1rn3x3_h1ah3x7g996649}

Ehk ktryy herq-ooizxetypd jjdcxnatoty ol f aordllvmlbkytc inahkw socjgex, bls sfoe gwzuti 1467 my Rjzn Hfetoxea Gqmexyt.

Tnj Gimjyèrk Htpnjc iy ysexjqoxj dosjeisjd cgqwej yse Gqmexyt Doxn ox Fwbkwei Inahkw.

Tn 1508, Ptsatsps Zwttnjxiax tnbjytki ehk xz-cgqwej ylbaql rkhea (g rltxni ol xsilypd gqahggpty) ysaz bzuri wazjc bk f nroytcgq nosuznkse ol yse Bnretèwp Cousex.

Gplrfdo’y xpcuso butvlky lpvjlrki tn 1555 gx l cuseitzltoty ol yse lncsz. Yse rthex mllbjd ol yse gqahggpty fce tth snnqtki cemzwaxqj, bay ehk fwpnfmezx lnj yse osoed qptzjcs gwp mocpd hd xegsd ol f xnkrznoh vee usrgxp, wnnnh ify bk itfljcety hizm paim noxwpsvtydkse.
```

文章の中央付近に、フラグのような文字列が記載されているのが分かります。  

```
hgqqpohzCZK{m311a50_0x_a1rn3x3_h1ah3x7g996649}
```

始めの4文字は良く分からないのですが、「pohzCZK」は「picoCTF」になると考えられます。  
このようなフラグのフォーマットが見られる場合、換字式暗号を使っているものと推測できます。  
そこで、[CyberChef](https://gchq.github.io/CyberChef/)を利用して変換させてみました。  
ROT13のレシピでブルートフォースを試してみました。しかし、フラグになるような文字列は得られませんでした。  
そのため、ヴィジュネル暗号ではないかと考えて変換させてみます。  

### ヴィジュネル暗号
[ヴィジュネル暗号（Wikipediaのページ）](https://ja.wikipedia.org/wiki/%E3%83%B4%E3%82%A3%E3%82%B8%E3%83%A5%E3%83%8D%E3%83%AB%E6%9A%97%E5%8F%B7)が参考になります。  
この変換表を使って、暗号文から平文を導き出します。  

暗号文と平文の「picoCTF」部分を対応付けて見ると、1文字目がpで同じ文字、5文字目がCで同じ文字なので、4文字の周期性があるようです。これで、暗号キーは4文字ではないかと判断できます。  

今度は、[CyberChef](https://gchq.github.io/CyberChef/)のVigenere Decodeレシピを利用し、1文字ずつ入力して文字列が「picoCTF」になる変化を確認していきます。すると、キーが「agfl」になったときに、フラグ文字列が表示されました。  

<br>
<br>
<br>
<br>

## フラグ
> picoCTF{b311a50_0r_v1gn3r3_c1ph3r7b996649}

<br>
<br>

## 補足
復号全文  
```
It is interesting how in history people often receive credit for things they did not create

During the course of history, the Vigenère Cipher has been reinvented many times

It was falsely attributed to Blaise de Vigenère as it was originally described in 1553 by Giovan Battista Bellaso in his book La cifra del. Sig. Giovan Battista Bellaso

For the implementation of this cipher a table is formed by sliding the lower half of an ordinary alphabet for an apparently random number of places with respect to the upper halfpicoCTF{b311a50_0r_v1gn3r3_c1ph3r7b996649}

The first well-documented description of a polyalphabetic cipher however, was made around 1467 by Leon Battista Alberti.

The Vigenère Cipher is therefore sometimes called the Alberti Disc or Alberti Cipher.

In 1508, Johannes Trithemius invented the so-called tabula recta (a matrix of shifted alphabets) that would later be a critical component of the Vigenère Cipher.

Bellaso’s second booklet appeared in 1555 as a continuation of the first. The lower halves of the alphabets are now shifted regularly, but the alphabets and the index letters are mixed by means of a mnemonic key phrase, which can be different with each correspondent.
```
